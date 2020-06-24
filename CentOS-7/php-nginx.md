# Install PHP

PHP là thành phần thiết lập sẽ xử lý mã để hiện thị nội dung động. Nó có thể chạy các tập lệnh, kết nối cơ sở dữ liệu MySQL để lấy thông tin và trao đổi nội dung được xử lý cho máy chủ web của chúng tôi để hiển thị.

Cần cài đặt kho lưu trữ bên thứ ba để PHP được cài đặt dễ dàng trên máy chủ CentOS-7. REMI là kho lưu trữ gói phổ biến cung cấp các bản phát hành PHP mới nhất cho CentOS.

Cài đặt kho Remi cho CentOS 7,

`yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm`

Cài đặt PHP 7.4, Hiện là phiên bản ổn định của PHP. Để kích hoạt gói:

```
yum install -y yum-utils
yum-config-manager --enable remi-php74


# Các Option cài đặt
yum -y install php php-opcache php-mysql php-mysqlnd php-fpm
```

Sao lưu file cấu hình trước khi chỉnh sửa

 `cp /etc/php-fpm.d/www.conf /etc/php-fpm.d/www.conf.backup`

Mở tệp `/etc/php-fpm.d/www.conf` cấu hình bằng trình soạn thảo:

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

`listen = var/run/php-fpm/php-fpm.sock;`

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
