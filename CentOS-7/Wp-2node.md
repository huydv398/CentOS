## WordPress 2 Server 

## Các Bước cài đặt WordPress trên 2 Server 
## Chuẩn bị
* IP planning
* Mô hình Lab
* Tiến hành tắt tường lửa và selinux trên cả 2 máy Web và Sql :

`

## Tại máy Database 
### Cài đặt dịch vụ MySQL 

```
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum install mysql-server
```

Khởi động lại dịch vụ MySQL 

`systemctl start mysqld `

Cài lại mật khẩu cho root của cơ sở dữ liệu:

`mysql_secure_installation`

### Tạo cơ sở dữ liệu và người dùng MySQL cho WordPress 

Đăng nhập vào MySQL bằng tài khoản root:

`mysql -u root`

Nhập mật khẩu cho root tạo ở trên và nó sẽ chuyển đến dấu nhắc của lệnh MySQL:

**Tạo user và database sử dụng cho WordPress**

Tạo tên cơ sở dữ liệu:

```
mysql> create database testwp;
Query OK, 1 row affected (0.00 sec)
```

>**Chú ý**: Mỗi câu lệnh đều phải kết thúc bằng dấu `;`

Tạo user và mật khẩu

```
mysql> CREATE USER huydv@192.168.20.2 IDENTIFIED BY 'HUYdv398';
Query OK, 0 rows affected (0.00 sec)
```
* `192.168.20.2` : Đại chỉ của máy Web truy cập đến MySQL 
* `huydv` : Tên user để WordPress sử dụng để đăng nhập vào MySQL 
* `HUYdv398` : là mật khẩu của user: `Huydv`

Cài đặt quyền cho user để có quyền truy cập vào cơ sở dữ liệu.

```
mysql> GRANT ALL PRIVILEGES ON testwp.* TO huydv@192.168.20.2 IDENTIFIED BY 'HUYdv398';
Query OK, 0 rows affected (0.00 sec)
```

Thực hiện lệnh để cập nhật thay đổi.
```
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)
```

Thoát bằng câu lệnh `exit`.

## Tại máy Web
### Cài đặt Apache 
Apache là một phần mềm server http phổ biển.

Để cài đặt Apache ta sử dụng câu lệnh sau:

`yum -y install httpd`

Khởi động dịch vụ và cho phép nó khởi động cùng hệ thống:

```
systemctl start httpd
systemctl enable httpd
```

### Cài PHP

dịch vujPHP để chạy các tập lệnh, kết nối với cơ sở dữ liệu MySQL để lấy thông tin và đưa nội dung được sử lý đến trang web server để hiển thị.

Cập nhập kho lưu trữ EPEL và Remi cho hệ thống CentOS 7 bằng các lệnh dưới đây:
```
yum install epel-release

yum update epel-release

rpm -Uvh https://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

Cập nhật phiên bản cho PHP
```
yum-config-manager --enable remi-php73
yum -y install php php-opcache php-mysql
```

Tiến hành thêm thêm file sau để kiểm tra version PHP:

`echo "<?php phpinfo(); ?>" > /var/www/html/info.php`
Sau khi cài đặt thực hiện reset lại Apache 

`systemctl restart httpd`

### Tải và cài đặt WordPress 
Cài gói hỗ trợ `php-gd`:
`yum -y install php-gd`

Cài gói tải xuống `wget`
Tiến hành tải xuống Wordpress với phiên bản mới nhất;

`wget http://wordpress.org/latest.tar.gz` 

>**Lưu ý**: Bạn cần để ý tới thư mục mà wget tải xuống. ở đây mình đang đứng tại thư mục /root/

Show file vừa tải xuống

`ls | grep .tar.gz`

![Imgur](https://i.imgur.com/5aGXj5V.png)

>**Lưu ý**: giải nén file sẽ ra thư mục wordpress có đường dẫn /root/wordpress

Copy các file trong thư mục sau WordPress tới đường dẫn `/var/www/html` Như sau:

`cp -Rvf /root/wordpress/* /var/www/html`

### Cấu hình Wordpress

Di chuyển tới thư mục chứa Wordpress 

`cd /var/www/html`

File có cấu hình wp là file `wp-config.php`. Tuy nhiên ở đây chỉ có file `wp-config-sample.php`.Tiến hành cophy lại file cấu hình như sau:

`cp wp-config-sample.php wp-config.php`

Mở file `wp-config.php` với trình vi để sửa:

`vi wp-config.php`

Trong file tìm đến dòng dưới để chỉnh sửa:

```
/** Tên database cho WordPress */
define( 'DB_NAME', 'testwp' );

/** MySQL database tên Usern */
define( 'DB_USER', 'huydv' );

/** MySQL database password cho user */
define( 'DB_PASSWORD', 'HUYdv398' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );

/** Database Charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );
```

Lưu và thoát khỏi trình vi.

Khởi động lại Httpd

`systemctl restart httpd`

### Hoàn tất phần giao diện 

Trên trình duyệt, gõ địa chỉ IP server trên thanh url, trình duyệt sẽ xuất hiện như sau:

![Imgur](https://i.imgur.com/Uch0Fow.png)

Nhập đầy đủ thông rồi `Install WorPress `

![Imgur](https://i.imgur.com/UatyBYP.png)

Login để đăng nhập:

![Imgur](https://i.imgur.com/GwvsTk4.png)

Đây là bảng điều khiển quản trị của website:

![Imgur](https://i.imgur.com/SntjYhJ.png)

Bạn cần phân quyền thư mục wp cho user apache để user này được phép tạo ra các thư mục và lưu trữ các tệp tin tải lên.
```
chmod -R 755 /var/www/html/*
chown -R apache:apache /var/www/html/*
```

Như vậy đã xong các bước cài đặt Wordpress trên 2 Server. Chúc các bạn thực hiện thành công. Và cám ơn các bạn đã xem.

