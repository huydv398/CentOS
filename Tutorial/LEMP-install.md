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

`yum install -y update`

`yum install -y epel-release`


* Cài đặt `nginx`

`yum install nginx -y`

* Khởi động nginx:

`systemctl start nginx`

Nếu bạn đang chạy tường lửa, hãy chạy lệnh sau để cho phép lưu lượng HTTP và HTTPS:

```
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --reload
```
Tại thanh URL nhập địa chỉ Domain hoặc IP:

![Imgur](https://i.imgur.com/aI9lpvf.png)

Cài đặt thành công Nginx.

* Để cho phép Nginx khởi động cùng server :

`systemctl enable nginx`

## Cài đặt CSDL

## Cài đặt PHP

* PHP là thành phần thiết lập nội dung quan trọng.

* Cài đặt kho Remi cho CentOS-7:


`yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm`

* Sau khi cài đặt xong, bạn sẽ cần chạy một lệnh để kích hoạt kho lưu trữ chứa phiên bản PHP:

`yum --disablerepo="*" --enablerepo="remi-safe" list php[7-9][0-9].x86_64`

Cài đặt PHP 7.4 :

`yum install -y yum-utils`

`yum install php php-mysqlnd php-fpm`

Kiểm tra lại php:

`php --version`

Mở tệp cấu hình `/etc/php-fpm.d/www.conf` tệp cấu hình và chỉnh sửa:

`vi /etc/php-fpm.d/www.conf`

Xác định vị trí lệnh `listen`, Theo mặc định `php-fpm` sẽ lắng nghe trên một máy chủ và cổng cụ thể qua TCP. Thay đổi cài đặt để nó lắng nghe trên socket file, vì điều này giúp cải thiện hiệu suất tổng thể của máy chủ.


Thay đổi cài đặt của chủ sở hữu và nhóm cho tệp. Xác định vị trí lệnh `listen.owner`, `listen.group`, `listen.mode`. Loại bỏ dấu `;` dấu trước ở đầu dòng. Sau đó thay đổi thành nginx.

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

`sudo systemctl restart nginx`

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


