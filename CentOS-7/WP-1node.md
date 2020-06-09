# Wordpress
* Là hệ thống quản lý nội dung miễn phí và mã nguồn mở xây dựng dựa trên PHP và MySQL. Được phát hành vào năm 2003.

## Các bước cài đặt
### Chuẩn bị 
Trước khi cài đặt WP, phải tiến hành bộ cài đặt LAMP trên máy server.
[Hướng dẫn cài đặt LAMP](https://github.com/huydv398/Linux-tutorial/blob/master/CentOS-7/LAMP.md)
### Tạo cơ sở dữ liệu cho tài khoản Wordpress 
* Đăng nhập vào tài khoản root của cơ sở dữ liệu:

`mysql -u root -p`

Bạn cần nhập Password mà bạn đã thiết lạp cài đặt khi cài đặt Mariadb. Khi nhập xong sẽ chuyển sang màn hình Mariadb .

Tiếp theo thiết lập bạn sẽ tạo cơ sở dữ liệu cho wordpress. Có thể dùng tên bất kỳ cho tên của Database.
Trong bài mình đặt là **testwp**.

`CREATE DATABASE testwp;`

Bạn cần tạo một tài khoản riêng để quản lý cơ sở dữ liệu cho Wordpress. Trong bài sẽ đặt 
* user: huydv
* password: HUYdv398

    `CREATE USER huydv@localhost IDENTIFIED BY 'HUYdv398';`

Tiến hành cấp quyền quản lý CSDL Wordpress cho user mới tạo:

`GRANT ALL PRIVILEGES ON wordpress.* TO huydv@localhost IDENTIFIED BY 'HUYdv398';`

Xác thực lại những thay đổi về quyền: 

`FLUSH PRIVILEGES;`

Hoàn tất và thoát khỏi Mariadb:

`exit`

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

`tar xvfz latest.tar.gz`

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

Trên là hướng dẫn cài đặt WordPress trên 1 Server - All in One. Mời bạn đọc tiếp phần Wordpress cài đặt Wordpress trên 2 Server [tại đây](CentOS-7/Wp-2node.md)