Lab Nginx Reverse Proxy & Caching

## Giới thiệu

Web cache (proxy server ) là gì tìm hiểu [tại đây](Fundamental/web-cache.md)

Về Cache: Nhiệm vụ chung của cache giúp tăng tốc độ truy cập dữ liệu và giảm tắc nghẽn về băng thông khi có quá nhiều người dùng cung truy cập đồng thời vào dữ liệu web.

Với Nginx đóng vai trò là cache: Kỹ thuật Cache trong Nginx được sử dụng để việc cải tiến tốc độ truy cập web hoặc ứng dụng trong các giải pháp CDN(Content Delivery Network)

Mô hình

Trong mô hình thực hiện các cấu hình sau:

1. Cài đặt máy web server sử dụng Apache, sau đó up nội dung các file tạo site đơn giản
2. Cài đặt Nginx làm chức năng Reverse Proxy và Caching.
3. Người dùng mở trình duyệt hoặc dùng lệnh `curl` với tùy chọn `-l` để kiểm tra xem proxy có hoạt động hay không.
4. Người dùng truy cập nhiều lần vào web hoặc dùng `curl` để kiểm tra xem đã cache được chưa?

## Triển khai
Tiến hành tắt tường lửa và SElinux trên cả 2 máy:

```
systemctl stop firewalld
setenforce 0
```
### Cài đặt Apache trên máy web-server 

Thiết lập IP theo mô hình.

Thực hiện trên máy 2.2.2.3/24

Thực hiện cài đặt các gói bổ trợ với quyền root

```
yum update -y
yum install -y epel-release 
yum install -y wget byobu 
```
Cài đặt Apache:

Cài đặt HTTPD:

`yum install -y httpd`

Khởi động dịch vụ và kích hoạt khi máy khởi động lại

```
systemctl start httpd
systemctl enable httpd
```

Kiểm tra trạng thái hoạt động

`systemctl status httpd`

Tạo một trang web có nội dung hình ảnh hoặc file tĩnh

```
vi /var/www/html/index.html
```

Thêm nội dung cho file
```
<html>
<h1> Chao mung cac ban cua huynet.com </h1>
<body><blockquote class="imgur-embed-pub" lang="en" data-id="a/4R8plN6"><a href="//imgur.com/a/4R8plN6">Forging steel</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script></body><br>
<img src = "https://i.imgur.com/pB0B5VX.png" alt = "HTML Tutorial" /><br>
<img src = "https://i.imgur.com/jRbjzax.jpg" alt = "HTML Tutorial" />

</html>
```

Lưu và thoát file.

Mở trình duyệt và truy cập địa chỉ `2.2.2.3` hiện thị nội dung như sau:

![Imgur](https://i.imgur.com/60iUwU2.png)

### Cài đặt Nginx

Cập nhật và cài đặt các tùy chọn:

```
yum update -y
yum install -y epel-release 
yum install -y wget byobu 
```

Cài đặt cấu hình Nginx từ Package:

`yum install -y nginx`

Khởi động Nginx và kích hoạt chế độ khởi động cùng server:

```
systemctl start nginx
systemctl enable nginx
```

Kiểm tra hoạt động của Nginx"

` systemctl status nginx`

### Cấu hình Nginx làm reverse proxy

Sao lưu cấu hình Nginx.conf:

`cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup`

Cấu hình Nginx:

Khi báo file cấu hình:

`vi /etc/nginx/conf.d/huynet.com.conf`

Nội dung như sau:

```
server {
    listen 80;
    server_name huynet.com;
    access_log /var/log/nginx/huynet.access.log;
    error_log /var/log/nginx/huynet.error.log;
    
    location / {
        proxy_pass http://2.2.2.3:80/;
    }
}
```

Lưu và thoát file.

Sau khi khai báo xong, kiểm tra lại bằng câu lệnh:

` nginx -t`

Và hiển thị kết quả như sau:

![Imgur](https://i.imgur.com/JAt9v7H.png)

Khởi động lại file cấu hình Nginx:

`nginx -s reload`

hoặc

`systemctl restart nginx`

Đối với máy tôi đang sử dụng lab cấu hình là Window 10, trỉnh sửa file `hosts` để trỏ domain. Cụ thể mở file có đường dẫn `C:\Windows\System32\drivers\etc\hosts` để sửa file cấu hình như sau:

![Imgur](https://i.imgur.com/2a9SwsS.png)


Kiểm tra nội dung web và xem reverse proxy đã hoạt động hay chưa bằng cách truy cập vào địa chỉ `huynet.com` trên thanh URL. Trong ảnh này hiển thị được nội dung của Web-server.
![Imgur](https://i.imgur.com/AIWCv20.png)

![Imgur](https://i.imgur.com/SdgQIvu.png)

Hoặc có thể sử dụng lệnh `curl -I` để kiểm tra
![Imgur](https://i.imgur.com/7M6w5tG.png)

### Cấu hình caching cho Nginx

Khai thêm các tùy chọn để biến Ngĩn trở thành máy chủ cache

Tạo thư mục chứa các file chứa cache:

```
mkdir -p /var/lib/nginx/cache
chown nginx /var/lib/nginx/cache
chmod 700 /var/lib/nginx/cache

```
Mở file huynet.com.conf ở trên và thêm các dòng này vào phần **#main context**(nằm ở phần block ngoài cùng)

`vi /etc/nginx/conf.d/huynet.com.conf`

```
proxy_cache_path /var/lib/nginx/cache levels=1:2 keys_zone=backcache:8m max_size=50m;
proxy_cache_key "$scheme$request_method$host$request_uri$is_args$args";
proxy_cache_valid 200 302 10m;
proxy_cache_valid 404 1m;
```

Tiếp tục thêm dòng dưới vào directive `location`:

```
proxy_cache backcache;
add_header X-Proxy-Cache $upstream_cache_status;
```

Kiểm tra khai bào cache ở trên bằng lệnh nginx -t, kết quả như sau:

```
proxy_cache_path /var/lib/nginx/cache levels=1:2 keys_zone=backcache:8m max_size=50m;
proxy_cache_key "$scheme$request_method$host$request_uri$is_args$args";
proxy_cache_valid 200 302 10m;
proxy_cache_valid 404 1m;

server {
    listen 80;
    server_name huynet.com;
    access_log /var/log/nginx/huynet.access.log;
    error_log /var/log/nginx/huynet.error.log;

    location / {
        proxy_pass http://2.2.2.3:80/;
        proxy_cache backcache;
        add_header X-Proxy-Cache $upstream_cache_status;

        }
}

```

Thực hiện khởi động lại Nginx bằng câu lệnh:

`nginx -s reload`

hoặc 

` systemctl restart nginx`

Kiểm tra lại cache đã hoạt động hay chưa

Đứng trên máy client thực hiện

* Sử dụng lệnh `curl` : đứng từ máy client vào web hoặc dùng công cụ MobaXterm để thông qua lệnh `curl` thực hiện truy cập vào website. Ta thực hiện request 2 lần và quan sat với dòng `X-proxy-cache` sẽ thấy trạng thái `MISS` hoặc `HIT`:


![Imgur](https://i.imgur.com/7m64AKP.png)

* Sử dụng trình duyệt:

Lần 1:


![Imgur](https://i.imgur.com/EmpSzF7.png)

Lần 2:

![Imgur](https://i.imgur.com/OFENDJ4.png)

Kiểm tra file trên máy chủ Nginx
* Truy cập vào trong máy chủ Nginx và kiểm tra thư mục cache, ta sẽ thấy file được hash, đó chính là nội dung của web đã được cache, sử dụng lệnh : `du -ah /var/lib/nginx/cache/`

