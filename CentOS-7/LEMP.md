# Cài đặt LEMP trên CentOS-7.

LEMP là một nhóm các phần mềm có thể phục vụ các trang web động và các ứng dụng web.

* **Linux**: Là hệ điều hành, cũng là phần mềm dùng để điều phối và quản lý các tài nguyên của hệ thống.
* **(E)nginx**: Là phần mềm máy chủ web, có thể thực hiện các request(Yêu cầu) được gọi tới máy chủ thông qua các giao thức HTTP.
* **MySQL** & **MariaDB**: Là hệ quản trị cở sở dữ liệu giúp lưu trữ và truy xuất dữ liệu.
* **PHP**: là ngôn ngữ lập trình cho kịch bản hoạt động của máy chủ.

## Chuẩn bị
* Máy được cài sẵn hệ điều hành CentOS-7
* Lệnh được dùng quyền root.
* Có kết nối ra môi trường Internet

## Cài đặt Nginx
```
yum -y update
yum install -y epel-release
```


* Cài đặt và khởi động `nginx`

```
yum install nginx -y
systemctl start nginx
```

Nếu bạn đang chạy tường lửa, hãy chạy lệnh sau để cho phép lưu lượng HTTP và HTTPS:

```
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --reload
```
Tại thanh URL nhập địa chỉ Domain hoặc IP:

![Imgur](https://i.imgur.com/f60zmTC.png)

Cài đặt thành công Nginx.

* Để cho phép Nginx khởi động cùng server :

`systemctl enable nginx`

## Cài đặt CSDL

Bạn có thể sử dụng MySQL hoặc Mariadb.

* Trên cửa sổ Terminal, tiến hành cài Mariadb:

`yum -y install mariadb mariadb-server `

* Tiến hành khởi động mariadb service: 

`systemctl start mariadb `

* Cài lại mật khẩu cho root của cơ sở dữ liệu:

`mysql_secure_installation`

* Thiết lập một số cấu hình tại bước này như sau:

* **Enter** khi đã có mật khẩu:

![Imgur](https://i.imgur.com/0kUCDRx.png)

* **Enter** Khi chưa có mật khẩu:

![Imgur](https://i.imgur.com/Jl2d0je.png)

![Imgur](https://i.imgur.com/M3zy1lG.png)

1: Nhấn `y` để cài mật khẩu mới cho root

 2: Nhập mật khẩu mới cho root

 3: Nhập lại mật khẩu cho root

* Nhập `y` xóa bỏ các User khác:

![Imgur](https://i.imgur.com/o5QZlJI.png)

* Không cho phép root đăng nhập từ xa:

![Imgur](https://i.imgur.com/JuAa7ap.png)

* Xóa bỏ database:

![Imgur](https://i.imgur.com/7OLrFFG.png)

* Khởi chạy lại bảng Privileges(Bảng phân quyền)

![Imgur](https://i.imgur.com/252vBxj.png)

Sau khi thiết lập, Kích hoạt mariadb để khởi động cùng hệ thống
## Cài đặt PHP

* PHP là thành phần thiết lập nội dung quan trọng.

* Cài đặt kho Remi cho CentOS-7:


`yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm`

* Sau khi cài đặt xong, bạn sẽ cần chạy một lệnh để kích hoạt kho lưu trữ chứa phiên bản PHP:

`yum --disablerepo="*" --enablerepo="remi-safe" list php[7-9][0-9].x86_64`

Cài đặt PHP 7.4 :
```
yum install -y yum-utils
yum-config-manager --enable remi-php74
yum install -y php php-mysqlnd php-fpm
```
Kiểm tra lại php:

`php --version`

### Cấu hình file `www.conf`
Sửa File
Mở tệp cấu hình `/etc/php-fpm.d/www.conf` và chỉnh sửa:

`vi /etc/php-fpm.d/www.conf`

Tìm kiếm các dòng lệnh sau và thay `apache` => `nginx`
```
user = apache
group = apache
```
Sửa thành 
```
user = nginx
group = nginx
```

Xác định vị trí lệnh `listen`, Theo mặc định `php-fpm` sẽ lắng nghe trên một máy chủ và cổng cụ thể qua TCP. Thay đổi cài đặt để nó lắng nghe trên socket file, vì điều này giúp cải thiện hiệu suất tổng thể của máy chủ.

Tìm đến dòng có chứa câu lệnh `listen = 127.0.0.1:9000`

Và sửa thành

`listen = /var/run/php-fpm/php-fpm.sock;`

Thay đổi cài đặt của chủ sở hữu và nhóm cho tệp. Xác định vị trí lệnh `listen.owner`, `listen.group`, `listen.mode`. Loại bỏ dấu `;` dấu trước ở đầu dòng. Sau đó thay đổi thành nginx.
```
;listen.owner = nobody  => listen.owner = nginx
;listen.group = nobody  => listen.group = nginx
;listen.mode = 0660     => listen.mode = 0660
```

Kích hoạt và khởi động `php-fpm`

`systemctl start php-fpm` 

Môi trường PHP đã sẵn sàng. 

## Cấu hình Nginx để xử lý các trang PHP

Nginx có một thư mục chuyên dụng nơi xác định mỗi trang web được lưu trữ dưới dạng cấu hình riêng biệt, sử dụng khối server. Điều này tương tự với các máy chủ ảo Apache.

Tạo một tệp để phục vụ như trang web PHP mặc định trên máy chủ này, sẽ ghi đè khối server mặc định được xác định trong tệp `/etc/nginx/nginx.conf`

Mở tệp mới trong thư mục `/etc/nginx/conf.d`:

` vi /etc/nginx/conf.d/default.conf`

Sao chép khối định nghĩa máy chủ PHP sau vào tệp cấu hình và thay thế lệnh `server_name` để nó trỏ đến tên miền hoặc địa chỉ IP của máy chủ:

```
server {
    listen       80;
    server_name  [server_domain hoặc IP];

    root   /usr/share/nginx/html;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;

    location = /50x.html {
        root /usr/share/nginx/html;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}

```

Lưu và đóng tệp khi hoàn tất.

Khởi động lại nginx:

`systemctl restart nginx`

## Kiểm tra xử lý PHP trên máy chủ Web của bạn

Web server đã được thiết lập, tạo tệp lệnh PHP thử nghiệm để đảm bảo Nginx xử lý chính xác tệp lệnh `.php` với sự trợ giúp `php-fpm`.

Tạo một tệp PHP mới được gọi là `info.php` tại thư mục `/usr/share/nginx/html`:

`vi /usr/share/nginx/html/info.php`

Mã PHP sau đây sẽ hiển thị thông tin về môi trường PHP hiện tại đang chạy trên máy chủ:

```
<?php
phpinfo();
?>
```

Khi bạn hoàn thành, lưu và đóng tệp

Kiểm tra tại trình duyệt

`http://server_host_or_IP/info.php`

Hiển thị kết quả như sau:

![Imgur](https://i.imgur.com/tICTazD.png)
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

`GRANT ALL PRIVILEGES ON testwp.* TO huydv@localhost IDENTIFIED BY 'HUYdv398';`

Xác thực lại những thay đổi về quyền: 

`FLUSH PRIVILEGES;`

Hoàn tất và thoát khỏi Mariadb:

`exit`

## Tải và cài đặt WordPress 
Cài gói hỗ trợ `php-gd`:
`yum -y install php-gd`

Cài gói tải xuống `wget`
Tiến hành tải xuống Wordpress với phiên bản mới nhất;

```
yum install wget -y
wget http://wordpress.org/latest.tar.gz
``` 

>**Lưu ý**: Bạn cần để ý tới thư mục mà wget tải xuống. ở đây mình đang đứng tại thư mục /root/

Giải nén tệp tin.

`tar xvfz latest.tar.gz`

>**Lưu ý**: giải nén file sẽ ra thư mục wordpress có đường dẫn /root/wordpress

Copy các file trong thư mục sau WordPress tới đường dẫn `/usr/share/nginx/html/` Như sau:

`cp -Rvf /root/wordpress/* /usr/share/nginx/html/`

### Cấu hình Wordpress

Di chuyển tới thư mục chứa Wordpress 

`cd /usr/share/nginx/html/`

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

Khởi động lại nginx

`systemctl restart nginx`

### Hoàn tất phần giao diện 

Trên trình duyệt, gõ địa chỉ IP server trên thanh url, trình duyệt sẽ xuất hiện như sau:



Nhập đầy đủ thông rồi `Install WorPress `

![Imgur](https://i.imgur.com/CfbmgAN.png)



Login để đăng nhập:

![Imgur](https://i.imgur.com/UatyBYP.png)



Đây là bảng điều khiển quản trị của website:

![Imgur](https://i.imgur.com/LdHCyLK.png)

