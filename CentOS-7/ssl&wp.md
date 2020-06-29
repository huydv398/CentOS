# Hướng dẫn cấu hình Nginx với SSL làm Reverse Proxy cho WordPress trên Server CentOS-7

Mô hình:

![Imgur](https://i.imgur.com/EzViV4V.png)

IPPlanning:

![Imgur](https://i.imgur.com/c5JQig7.png)

Yêu cầu: 
* Thực hiện quyền root với các câu lệnh.

Máy Server Nginx:
* Là một máy chủ Reverse Proxy.
* Cài đặt chứng chỉ SSL cho các website.

Máy Webserver :
* Cài đặt stack LAMP và cài đặt Wordpress.
* Chuyển hướng trang web về tên miền.

## Cấu hình trên Server Nginx

### Cài đặt Nginx

```
yum update
yum install -y nginx
```

Cho phép lưu lượng truy cập trên cổng 80.

```
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --reload
```

Backup file cấu hình

`cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup`

### Tạo file config site

`vi /etc/nginx/conf.d/wp.duonghuy.xyz`

Thêm nội dung sau vào file:
```
server {
server_name wp.duonghuy.xyz;
    location / {
        proxy_set_header   X-Real-IP             $remote_addr;
        proxy_set_header   X-Forwarded-For     $proxy_add_x_forwarded_for;

        proxy_set_header X-Forwarded-Proto https;
        proxy_pass http://10.10.35.115;
    }
} 
```

Kiểm tra lại file cấu hình

`nginx -t`

Nếu không có lỗi, Restart lại Nginx:

`nginx -s reload`

#### Kiểm tra site.

Vào trình duyệt để kiểm tra web. Nhập vào trình duyệt URL vừa cấu hình bên trên:

https://wp.duonghuy.xyz 

![Imgur](https://i.imgur.com/i7FhQO2.png)

#### Cài đặt chuyển hướng về tên miền

Tuy đã thành công truy cập thành công bằng tên miền, nhưng khi ta truy cập địa chỉ IP vẫn hiện địa chỉ IP trên thanh URL. Để chuyển về tên miền ta thực hiện như sau: 
* Truy cập và đăng nhập vào **Dashboard Admin**, vào tab **Settings** và chọn **General**.

* Sau đó chỉnh sửa mục **WordPress Address (URL)** và mục **Site Address (URL)**, thành địa chỉ URL mới là *https://wp.duonghuy.xyz*
* Chọn **Save Changes** để lưu lại cài đặt

## Thiết lập chứng chỉ Let's Encrypt

Cài đặt Certbot 

`yum install python-certbot-nginx -y`

Sinh SSL bằng Let's Encrypt cho site *wp.duonghuy.xyz*

`certbot --nginx -d wp.duonghuy.xyz`

**OUTPUT**
```
[root@nginxlab ~]# certbot --nginx -d wp.duonghuy.xyz
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator nginx, Installer nginx
Starting new HTTPS connection (1): acme-v02.api.letsencrypt.org
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for wp.duonghuy.xyz
Waiting for verification...
Cleaning up challenges
Deploying Certificate to VirtualHost /etc/nginx/conf.d/wp.duonghuy.xyz.conf
Redirecting all traffic on port 80 to ssl in /etc/nginx/conf.d/wp.duonghuy.xyz.conf

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations! You have successfully enabled https://wp.duonghuy.xyz

You should test your configuration at:
https://www.ssllabs.com/ssltest/analyze.html?d=wp.duonghuy.xyz
- - - - - - - - - - - - - - - - - - - - - - - - - - - - -                                                          - - - - - - - - - - -

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have beensaved at:
   /etc/letsencrypt/live/wp.duonghuy.xyz/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/wp.duonghuy.xyz/privkey.pem
   Your cert will expire on 2020-09-27. To obtain a new or tweaked
   version of this certificate in the future, simply runcertbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

Kiểm Tra lại kết quả trong file config site

Sau khi sinh ssl cho Site, ta kiểm tra lại file config sẽ thấy sự thay đổi.

`cat /etc/nginx/conf.d/wp.duonghuy.xyz.conf`


```
server {
 server_name wp.duonghuy.xyz;
     location / {
         proxy_set_header   X-Real-IP             $remote_addr;
         proxy_set_header   X-Forwarded-For     $proxy_add_x_forwarded_for;

         proxy_set_header X-Forwarded-Proto https;
         proxy_pass http://10.10.35.115;
     }


    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/wp.duonghuy.xyz/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/wp.duonghuy.xyz/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}
server {
    if ($host = wp.duonghuy.xyz) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


 server_name wp.duonghuy.xyz;
    listen 80;
    return 404; # managed by Certbot


}
```

Khởi động lại Nginx:

`nginx -s reload`

## Cấu hình trên webserver cài Wordpress

### Sửa file `wp-config.php`

Tiến hành vào file `wp-config.php` tại `/var/www/html`

`vi /var/www/html/wp-config.php`

Thêm vào dòng sau:
```
if ($_SERVER['HTTP_X_FORWARDED_PROTO'] == 'https'){ $_SERVER['HTTPS']='on'; } if (isset($_SERVER['HTTP_X_FORWARDED_HOST'])) { $_SERVER['HTTP_HOST'] = $_SERVER['HTTP_X_FORWARDED_HOST']; } 

```

![Imgur](https://i.imgur.com/6XMea6B.png)

Khởi động lai dịch vụ httpd

`systemctl restart httpd `

Truy cập trang web bằng địa chỉ *https://wp.duonghuy.xyz* 

![Imgur](https://i.imgur.com/aXQhYZD.png)

![Imgur](https://i.imgur.com/zOWQfFI.png)

### Kết luận 

Trên là phần hướng dẫn về cách cấu hình Nginx làm Reverse proxy cho WordPress trên CentOS-7

Tham khảo: https://news.cloud365.vn/huong-dan-cau-hinh-nginx-voi-ssl-lam-reverse-proxy-cho-wordpress-tren-ubuntu-server-18-04-lts/