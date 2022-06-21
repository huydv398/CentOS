# Apache
## Installing Apache
Apache có sẵn trong kho phần mềm mặc định của CentOS, có nghĩa là bạn có thể cài đặt nó bằng trình quản lý gói yum.
```
yum update httpd -y
yum install httpd -y
```


Cần mở cổng 80, 443 để cho phép Apache phục vụ các yêu cầu qua HTTP, HTTPS

```
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```

##  Checking your Web Server
Bạn sẽ cần bắt đầu quy trình Apache theo cách thủ công:
```
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```

Thực hiện kiểm tra bằng cách truy cập url http://IP-your-server 
## Setting Up Virtual Hosts
Khi sử dụng máy chủ web Apache, bạn có thể sử dụng máy chủ ảo (tương tự như khối máy chủ trong Nginx) để đóng gói chi tiết cấu hình và lưu trữ nhiều hơn một miền từ một máy chủ duy nhất. Trong bước này, bạn sẽ thiết lập một miền được gọi là **huydv.1lab.xyz**, nhưng bạn nên thay thế miền này bằng tên miền của riêng bạn. 

Apache trên CentOS 7 có một khối máy chủ được kích hoạt theo mặc định được cấu hình để cung cấp tài liệu từ thư mục **/var/www/html**. Mặc dù điều này hoạt động tốt cho một trang web, nhưng nó có thể trở nên khó sử dụng nếu bạn đang lưu trữ nhiều trang web. Thay vì sửa đổi **/var/www/html**, bạn sẽ tạo cấu trúc thư mục bên trong **/var/www** cho trang **your_domain**, giữ nguyên **/var/www/html** làm thư mục mặc định sẽ được cung cấp nếu yêu cầu của khách hàng không khớp bất kỳ trang web nào khác.

Tạo thư mục html cho **huydv.1lab.xyz** như sau, sử dụng cờ `-p` để tạo bất kỳ thư mục mẹ nào cần thiết:
```
sudo mkdir -p /var/www/your_domain/html
```

Tạo một thư mục bổ sung để lưu trữ các tệp nhật ký cho trang web:
```
sudo mkdir -p /var/www/your_domain/log
```

Tiếp theo, chỉ định quyền sở hữu thư mục html với biến môi trường `$USER`:
```
sudo chown -R $USER:$USER /var/www/your_domain/html
```

Đảm bảo rằng gốc web của bạn có các quyền mặc định được đặt:
```
sudo chmod -R 755 /var/www
```

Tiếp theo, tạo một trang `index.html` mẫu bằng vi hoặc trình soạn thảo yêu thích của bạn:
```
sudo vi /var/www/your_domain/html/index.html
```


THực hành:
```
sudo mkdir -p /var/www/huydv.1lab.xyz/html
sudo mkdir -p /var/www/huydv.1lab.xyz/log
sudo chown -R $USER:$USER /var/www/huydv.1lab.xyz/html
sudo chmod -R 755 /var/www
sudo vi /var/www/huydv.1lab.xyz/html/index.html
```

Thêm vào file index:
```
<html>
  <head>
    <title>Welcome huydv!</title>
  </head>
  <body>
    <h1>Success! Huydv xin chào tới đây!</h1>
  </body>
</html>
```

Lưu và thoát.

Trước khi tạo máy chủ ảo, bạn sẽ cần tạo một thư mục `sites-available` để lưu trữ chúng. Bạn cũng sẽ tạo thư mục `sites-enabled` thông báo cho Apache rằng máy chủ ảo đã sẵn sàng phục vụ khách truy cập. Thư mục `sites-enabled` sẽ chứa các liên kết tượng trưng đến các máy chủ ảo mà chúng tôi muốn xuất bản. Tạo cả hai thư mục bằng lệnh sau:
```
sudo mkdir /etc/httpd/sites-available /etc/httpd/sites-enabled
```

Tiếp theo, bạn sẽ yêu cầu Apache tìm kiếm các máy chủ ảo trong thư mục `sites-available`. Để thực hiện điều này, hãy chỉnh sửa tệp cấu hình chính của Apache và thêm một dòng khai báo thư mục tùy chọn cho các tệp cấu hình bổ sung:

```
sudo vi /etc/httpd/conf/httpd.conf
```

Thêm dòng sau vào cuối:
```
IncludeOptional sites-enabled/*.conf
```

lưu và thoát

Bắt đầu bằng cách tạo một tệp mới trong thư mục `sites-available`:
```
sudo vi /etc/httpd/sites-available/huydv.1lab.xyz.conf
```
Thêm vào khối cấu hình sau và thay đổi `huydv.1lab.xyz` thành tên miền của bạn:
```
<VirtualHost *:80>
    ServerName www.your_domain
    ServerAlias your_domain
    DocumentRoot /var/www/your_domain/html
    ErrorLog /var/www/your_domain/log/error.log
    CustomLog /var/www/your_domain/log/requests.log combined
</VirtualHost>
```
THực tế: 

Bấm `i` để vào chế độ Insert 
```

<VirtualHost *:80>
    ServerName www.huydv.1lab.xyz
    ServerAlias huydv.1lab.xyz
    DocumentRoot /var/www/huydv.1lab.xyz/html
    ErrorLog /var/www/huydv.1lab.xyz/log/error.log
    CustomLog /var/www/huydv.1lab.xyz/log/requests.log combined
</VirtualHost>
```
Lưu và thoát bấm `Esc` -> `:wq` 

Bây giờ bạn đã tạo các tệp máy chủ ảo, bạn sẽ kích hoạt chúng để Apache biết để phục vụ chúng cho khách truy cập. Để thực hiện việc này, hãy tạo một liên kết tượng trưng cho từng máy chủ ảo trong thư mục `sites-enabled`:

```
sudo ln -s /etc/httpd/sites-available/your_domain.conf /etc/httpd/sites-enabled/your_domain.conf
```

Thực tế
```

sudo ln -s /etc/httpd/sites-available/huydv.1lab.xyz.conf /etc/httpd/sites-enabled/huydv.1lab.xyz.conf
```

Tạo host thứ 2 tương tự.

Sau khi tạo 2 host domain sẽ như sau:

![Imgur](https://i.imgur.com/AUYvOQc.png)

![Imgur](https://i.imgur.com/zrhQgId.png)
## Điều chỉnh quyền SELinux cho Máy chủ ảo
SELinux được cấu hình để hoạt động với cấu hình Apache mặc định. Vì bạn thiết lập một thư mục nhật ký tùy chỉnh trong tệp cấu hình máy chủ ảo, bạn sẽ gặp lỗi nếu bạn cố gắng khởi động dịch vụ Apache. Để giải quyết vấn đề này, bạn cần cập nhật các chính sách của SELinux để cho phép Apache ghi vào các tệp cần thiết. SELinux mang lại tính bảo mật cao hơn cho môi trường CentOS 7 của bạn, do đó bạn không nên tắt hoàn toàn kernel module.

Có nhiều cách khác nhau để đặt chính sách dựa trên nhu cầu của môi trường của bạn, vì SELinux cho phép bạn tùy chỉnh mức độ bảo mật của mình. Bước này sẽ bao gồm hai phương pháp điều chỉnh chính sách Apache: trên toàn cầu và trên một thư mục cụ thể. Điều chỉnh các chính sách trên thư mục là an toàn hơn, và do đó là cách tiếp cận được khuyến nghị.
```
sudo setsebool -P httpd_unified 1

sudo ls -dZ /var/www/your_domain/log/

sudo semanage fcontext -a -t httpd_log_t "/var/www/your_domain/log(/.*)?"

sudo restorecon -R -v /var/www/your_domain/log
sudo ls -dZ /var/www/your_domain/log/
```

Hoặc tắt SE linux
```
setenforce 0
```

## Cài đặt SSL
Điều kiện:
* Sử dụng Ip public
* Domain name (1lab.xyz)
* Server CentOS 7- cài Intial

### Installing the Certbot Let’s Encrypt Client
Để sử dụng Let’s Encrypt để lấy chứng chỉ SSL, trước tiên bạn cần cài đặt Certbot và `mod_ssl`, một mô-đun Apache cung cấp hỗ trợ cho mã hóa SSL v3.

Gói `certbot` không có sẵn thông qua trình quản lý gói theo mặc định. Bạn sẽ cần kích hoạt kho EPEL để cài đặt Certbot.

Để thêm kho lưu trữ CentOS 7 EPEL, hãy chạy lệnh sau:
```
sudo yum install epel-release
```

Bây giờ bạn có quyền truy cập vào kho lưu trữ, hãy cài đặt tất cả các gói được yêu cầu

```
sudo yum install certbot python2-certbot-apache mod_ssl
```
### Lấy chứng chỉ
Bây giờ Certbot đã được cài đặt, bạn có thể sử dụng nó để yêu cầu chứng chỉ SSL cho miền của mình.

Việc sử dụng ứng dụng khách certbot Let’s Encrypt để tạo Chứng chỉ SSL cho Apache sẽ tự động hóa nhiều bước trong quy trình. Máy khách sẽ tự động lấy và cài đặt chứng chỉ SSL mới hợp lệ cho các miền bạn cung cấp dưới dạng tham số.

Để thực hiện cài đặt tương tác và lấy chứng chỉ chỉ bao gồm một miền duy nhất, hãy chạy lệnh certbot với
```
sudo certbot --apache -d example.com
```
Thực tế:
```
sudo certbot --apache -d huydv.1lab.xyz -d huysun.1lab.xyz
``` 

output:
```
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/huydv.1lab.xyz/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/huydv.1lab.xyz/privkey.pem
   Your certificate will expire on 2022-09-19. To obtain a new or
   tweaked version of this certificate in the future, simply run
   certbot again with the "certonly" option. To non-interactively
   renew *all* of your certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:     
```
### Checking your Certificate Status

![Imgur](https://i.imgur.com/j8iRc5g.png)
