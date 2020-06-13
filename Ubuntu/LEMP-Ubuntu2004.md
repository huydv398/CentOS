## Cài đặt máy chủ web Nginx

Sử dụng trình quản lý gói  `apt` để cài đặt phần mềm này.

Sử dụng `apt  install ` để cài đặt Nginx:

`sudo apt update`

`sudo apt install nginx`

Khi được nhắc, nhập `y` để xác nhận cài đặt Nginx 
### Điều chỉnh tường lửa

Kích hoạt tính năng cho phép lưu lượng HTTP thông thường trên cổng 80:

`sudo ufw allow 'Nginx HTTP'`

### Kiểm tra máy chủ web 

Kiểm tra với câu lệnh :

`systemctl status nginx`

```
 nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset>
     Active: active (running) since Sat 2020-06-13 02:50:16 UTC; 10min ago
       Docs: man:nginx(8)
   Main PID: 2996 (nginx)
      Tasks: 2 (limit: 2249)
     Memory: 3.7M
     CGroup: /system.slice/nginx.service
             ├─2996 nginx: master process /usr/sbin/nginx -g daemon on; master>
             └─2997 nginx: worker process

```

Hoặc nhập địa chỉ IP máy chủ:

`http://[địa_chỉ_IP]`

![Imgur](https://i.imgur.com/aJVbSMY.png)

### Thiết lập khối máy chủ
Khi sử dụng Nginx web server, server blocks (tương tự máy chủ ảo trong Apache) có thể được sử dụng để đóng gói chi tiết cấu hình và lưu trữ nhiều tên miền từ một máy chủ.

Thiết lập một tên miền được gọi là `test_domain`.

Nginx trên Ubuntu 20.04 có một server block được bật theo mặc dịnh được  cấu hình để phấn phát tài liệu ra khỏi thư mục `/var/www/html`. Mặc dù hoạt động tốt trên một trang web nhưng nó trở nên khó sử dụng nếu bạn đang lưu trữ nhiều trang web.Thay vì sửa đổi `/var/www/html`, hãy tạo cấu trúc trang**test_domain.com** trong thư mục `var/www`

Tạo thư mục cho **test_domain.com**, Sử dụng flat `-p` để tạo bất kỳ thư mục cần thiết nào:

`sudo mkdir -p /var/www/test_domain.com/html`

Gắn quyền sở hữu thư mục với biến môi trường `$USER` :

`sudo chown -R $USER:$USER /var/www/test_domain.com/html`

Cho phép chủ sở hữu đọc, ghi và thực thi các tệp trong khi chỉ cấp quyền đọc và thực thi quyền cho các nhóm và những người khác, bạn có thể nhập lệnh sau:

`sudo chmod -R 755 /var/www/test_domain.com`

Tạo một `index,html` bằng cách sử dụng trình chỉnh sửa:

`vi /var/www/test_domain.com/html/index.html`

Bên trong, Thêm HTML mẫu như sau:

```
<html>
    <head>
        <title>Welcome to your_domain!</title>
    </head>
    <body>
        <h1>Thành công! <b>Domain</b> đang hoạt đông</h1>
    </body>
</html>
```

Lưu và đóng tệp.


Để Nginx phục vụ môi trường này, cần phải tạo *server block* với các mệnh lệnh chính xác. Thay vì trực tiếp sửa đổi tệp cấu hình mặc định hãy tạo một tệp mới tại:`/etc/nginx/sites-available/test_domain.com`

`sudo nano /etc/nginx/sites-available/test_domain.com`

Thêm cấu hình sau vào tệp, tương tự như mặc định, nhưng được cập nhật cho thư mục và tên miền mới:

```
server {
        listen 80;
        listen [::]:80;

        root /var/www/test_domain.com/html;
        index index.html index.htm index.nginx-debian.html;

        server_name test_domain.com www.test_domain;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

>**Lưu ý** Cập nhật cấu hình `root` cho thư mục mới và `server_name` tên miền.

Tiếp theo, hãy kích hoạt tệp bằng cách tạo một liên kết từ nó đến `sites-enabled` thư mục mà Nginx đọc khi khởi động:

`sudo ln -s /etc/nginx/sites-available/test_domain.com /etc/nginx/sites-enabled/`

Hai khối máy chủ hiện được bật và định cấu hình để đáp ứng các yêu cầu dựa tren chỉ thị listen và server_name chỉ thị:
* `test_domain.com`: Sẽ trả lời các yêu cầu cho `test_domain.com` và `www.test_domain.com`.
* `default`: Sẽ trả lời bất kỳ yêu cầu nào trên cổng 80 không khớp với hai khối còn lại

Để tránh sự cố nộ nhớ nhóm `hash` có thể phát sinh từ việc thêm máy chủ bổ sung, cần phải điều chỉnh giá trị tệp `/etc/nginx/nginx.conf`. Mở tệp:

`sudo vi /etc/nginx/nginx.conf`

Tìm `server_names_hash_bucket_size` chị thị và xóa `#`.
```
...
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}
...
```

Lưu và thoát

Tiếp theo, kiểm tra để đảm bảo rằng không có lỗi cú pháp trong bất kỳ tệp Nginx nào của bạn:

`sudo nginx -t`

Nếu không có vấn đề gì, hãy khởi động lại Nginx để kích hoạt các thay đổi của bạn:

`sudo systemctl restart nginx`

Kiểm tra bằng cách gõ : `http://test_domain.com`

