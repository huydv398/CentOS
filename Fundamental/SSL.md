# Giới thiệu
* SSL là chữ viết tắt của Secure Sockets Layer (Lớp Socket bảo mật). Một loại bảo mật giúp mã hóa liên lạc giữa web site và trình duyệt. Công nghệ này đang lỗi thời và hoàn toàn thay thế bởi TLS
* TLS - Transport Layer Security, Nó cũng giúp bảo mật thông tin truyền thông như SSL. Nhưng vì SSL không còn được phát triển nữa, nên TLS mới là thuật ngữ đúng tại thời điểm hiện tại.
* Let's Enscrypt là một tổ chức xác thực SSL, là một tổ chức phi lợi nhuận được thành lập với sự bảo trợ của các tổ chức lớn trên thế giới như Cisco, Akamai, Mozilla, Facebook,... với mục đích là cung cấp chứng chỉ SSL miễn phí cho mọi người giúp mọi website đều được mã hóa, tạo nên môi trường Internet an toàn hơn.
* HTTPS là phần mở rộng bảo mật của HTTP. Website được cài đặt chứng chỉ SSL/TLS có thể sử dụng giao thức HTTPS để thiết lập kết nối an toàn với server.

Có 2 cách tạo SSL: 
* Nhờ một tổ chức CA(Certification Authority) cấp cho người sử dụng, là tổ chức có độ tin cậy cao, được quyền cấp và chứng nhận SSL. Sẽ mất phí. 
* Selt-signed SSL: là server tự cấp, tự ký, tự xác thực(không an toàn và tin tưởng bằng nhờ bên thứ 3). Với cách này sẽ không mất phí.

Bài viết sẽ hướng dẫn nhận chứng chỉ SSL miễn phí từ Let's Scrypts và cài đặt SSL trên môi trường Nginx và Cent)S-7.

Tuy nhiên, chứng chỉ SSL trên môi trường Nginx có tác dụng trong vòng 90 ngày. Sau 90 ngày bạn sẽ cần update lại chứng chỉ.

## Chuẩn bị 
1 Server chạy hệ điều hành CentOS-7, đã cài LEMP Stack

## Thực hiện cài đặt

Cài đặt Certbot 

Certbot là công cụ dòng lệnh miễn phí giúp đơn giản hóa quy trình gia hạn chứng chỉ SSL từ Let't Encrypt và tự động kích hoạt HTTPS trên máy chủ của bạn. 

* Cài đặt các gói cần thiết

```
yum module python36
yum install -y gcc mod_ssl python3-virtualenv redhat-rpm-config augeas-libs libffi-devel openssl-devel 
```

* Tải về Certbot script 

`curl -O https://dl.eff.org/certbot-auto`

Sau khi tải xuống hoàn tất, di chuyển file `certbot-auto` tới thư mục `/usr/local./bin/certbot-auto`


```
mv certbot-auto /usr/local/bin/certbot-auto
chmod 0755 /usr/local/bin/certbot-auto
```

### Tạo virtualhost 

Tạo 1 file cấu hình virtual host(server block) cho tên miền **ssl.duonghuy.xyz**

`vi /etc/nginx/conf.d/ssl.duonghuy.xyz.conf`

Thêm nội dung sau vào file

```
server {
      server_name ssl.duonghuy.xyz;
      root /var/www/site/ssl.duonghuy.xyz;

      location / {
         index index.html index.htm index.php;
      }
}

```

Tạo 1 Thư mục để cài đặt tệp HTML của bạn 

`mkdir -p /var/www/site/ssl.duonghuy.xyz`

Thay đổi quyền với thư mục:

`chown -R nginx:nginx /var/www/site/ssl.duonghuy.xyz`

Đặt file HTML thử nghiệm vào thư mục gốc của tên miền

`vi /var/www/site/ssl.duonghuy.xyz/index.html`


Thêm nội dung như sau:

```
<html>
    <head>
        <title>test!</title>
    </head>
    <body>
        <h3>Thanh cong ssl.duonghuy.xyz dang hoat dong</h3>
    </body>
</html>

```

Lưu và thoát

Khởi động lại Nginx

`nginx -s reload`

Kiểm tra trang web 

![Imgur](https://i.imgur.com/HQONPsP.png)

Chú ý trang web chưa được cài chứng nhận SSL

### Tạo bản ghi DNS

* Truy cập vào công cụ quản lý DNS:

![Imgur](https://i.imgur.com/BfwUWfK.png)

>**Chú ý**: Bản ghi phải loại A hoặc AAAA

* Kiểm tra đường truyền DNS


Cài đặt: 

`yum install -y bind-utils`

Tra Domain

`nslookup [domain]`

![Imgur](https://i.imgur.com/pHLwCaR.png)

### Thiết lập nhận chứng chỉ miễn phí từ Let's Encrypy

Sử dụng câu lệnh `certboot` để tạo và cài đặt chứng chỉ Let's Encrypt.

`/usr/local/bin/certbot-auto --nginx`

**Out Put**

```
[root@nginxlab ~]# /usr/local/bin/certbot-auto --nginx
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator nginx, Installer nginx

Which names would you like to activate HTTPS for?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: ssl.duonghuy.xyz
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel): 1
Cert not yet due for renewal

You have an existing certificate that has exactly the same domains or certificate name you requested and isn't close to expiry.
(ref: /etc/letsencrypt/renewal/ssl.duonghuy.xyz.conf)

What would you like to do?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: Attempt to reinstall this existing certificate
2: Renew & replace the cert (limit ~5 per 7 days)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 1
Keeping the existing certificate
Deploying Certificate to VirtualHost /etc/nginx/conf.d/ssl.duonghuy.xyz.conf
Traffic on port 80 already redirecting to ssl in /etc/nginx/conf.d/ssl.duonghuy.xyz.conf

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations! You have successfully enabled https://ssl.duonghuy.xyz

You should test your configuration at:
https://www.ssllabs.com/ssltest/analyze.html?d=ssl.duonghuy.xyz
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/ssl.duonghuy.xyz/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/ssl.duonghuy.xyz/privkey.pem
   Your cert will expire on 2020-09-24. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot-auto
   again with the "certonly" option. To non-interactively renew *all*
   of your certificates, run "certbot-auto renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le

```


```
Which names would you like to activate HTTPS for?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: ssl.duonghuy.xyz
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel): 1
Cert not yet due for renewal
```

#### Tại bước này máy chủ yêu cầu chọn số của tên domain muốn cài SSL, tại đây có 1 domain chọn phím 1.

```
What would you like to do?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: Attempt to reinstall this existing certificate
2: Renew & replace the cert (limit ~5 per 7 days)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel):
```

Khi cài lại lần 2 thì mày chủ hỏi:
* 1: Có muốn cố gắng cài đặt lại không.
* 2: Gia hạn và thay thế chứng chỉ.


```
Congratulations! You have successfully enabled https://ssl.duonghuy.xyz
```

Đến đây đã cài đặt thành công SSL cho website.

### Redirect tất cả các truy vẫn tới HTTPS

Thêm vào file `ssl.duonghuy.xyz.conf`
```
server {
listen 80;
server_name ssl.duonghuy.xyz;
return 301 https://ssl.duonghuy.xyz$request_uri;
} 
```

* restart service Nginx:


`systemctl restart nginx`

Cấu hình Firewall

Cấu hình firewall cho phép các yêu cầu HTTP

```
firewall-cmd --permanent --add-port=443/tcp
firewall-cmd --reload
```

Xác nhận chứng nhận Let's Encrypt

Kiểm tra trên trình duyệt

![Imgur](https://i.imgur.com/aCtiPgu.png)

Kiểm tra chứng nhận SSL

Kiểm tra chứng nhận SSL của bạn tại website https://www.ssllabs.com/ssltest để biết bất kỳ vẫn đề nào và xếp hạng bảo mật của nó.

![Imgur](https://i.imgur.com/H0bZDVT.png)

Thiết lập gia hạn tự động
* Sử dụng lệnh:

echo "0 0,12 * * * root python -c 'import random; import time; time.sleep(random.random() * 3600)' && /usr/local/bin/certbot-auto renew" | sudo tee -a /etc/crontab > /dev/null

* Mô phỏng quá trình gia hạn chứng chỉ bằng lệnh bên dưới để đảm bảo quá trình gia hạn diễn ra suôn sẻ

`/usr/local/bin/certbot-auto renew --dry-run`

**OUTPUT**

```
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - -- - - - - - - - - - -
Processing /etc/letsencrypt/renewal/demo.duonghuy.xyz.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - -- - - - - - - - - - -
Cert not due for renewal, but simulating renewal for dry run
Plugins selected: Authenticator nginx, Installer nginx
Renewing an existing certificate
Performing the following challenges:
http-01 challenge for demo.duonghuy.xyz
Waiting for verification...
Cleaning up challenges

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
new certificate deployed with reload of nginx server; full chain is
/etc/letsencrypt/live/demo.duonghuy.xyz/fullchain.pem
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/ssl.duonghuy.xyz.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Cert not due for renewal, but simulating renewal for dry run
Plugins selected: Authenticator nginx, Installer nginx
Renewing an existing certificate
Performing the following challenges:
http-01 challenge for ssl.duonghuy.xyz
Waiting for verification...
Cleaning up challenges

- - - - - - - - - - - - - - - - - - - - - - - - - - - - -- - - - - - - - - - -
new certificate deployed with reload of nginx server; fullchain is
/etc/letsencrypt/live/ssl.duonghuy.xyz/fullchain.pem
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

- - - - - - - - - - - - - - - - - - - - - - - - - - - - -- - - - - - - - - - -
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates below have not been saved.)

Congratulations, all renewals succeeded. The following certs have been renewed:
  /etc/letsencrypt/live/demo.duonghuy.xyz/fullchain.pem (success)
  /etc/letsencrypt/live/ssl.duonghuy.xyz/fullchain.pem (success)
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates above have not been saved.)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - -- - - - - - - - - - -

IMPORTANT NOTES:
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.

```

Tiến trình chạy bình thường không xảy ra lỗi là OK.

