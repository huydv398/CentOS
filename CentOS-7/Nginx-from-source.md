# Cài đặt Nginx từ mã nguồn trên CentOS-7.

Nginx là một máy chủ web hiệu suất cao, Mã nguồn mở, cung cấp một số nội dung tĩnh bằng cách sử dụng tài nguyên hệ thống. Nó cũng lưu trữ một số trang web lưu lượng truy cập internet cao nhất

Để cài đặt Nginx hoàn thiện từ source, Nginx cần những yêu cầu bắt buộc:

* Thư viện **OpenSSL**
* Thư viện **Zlib**
* Thư viện **PCRE**
* Trình biên dịch **GCC**

Các yêu cầu tùy chọn:
* PERL
* libaromic_ops
* LibGD
* MaxMind GeoIP
* libxml2
* libxslt

## Build Nginx from source

Sử dụng câu lệnh với quyền root

Cập nhật hệ thống:

`yum check-update || yum update -y`

Cài đặt Development libreries

Nếu bạn muốn cài đặt tất cả các thư viện Development bằng một lệnh:

`yum groupinstall " Development Tools"  -y`

Lệnh trên sẽ tải xuống và cài đặt tất cả các thư viện phát triển cần thiết vào hệ thống của bạn

Cài đặt các thư viện cần thiết, các gói được đề cập bên trên để biên dịch Nginx từ mà nguồn của nó.

`yum install zlib-devel pcre-devel openssl-devel -y`

Cài đặt kho Epel repository cho hệ thống:

`yum install epel-release -y`

Cài các yêu cầu tùy chọn để Nginx có thể chạy trên hệ thống.

`yum install perl perl-devel perl-ExtUtils-Embed libxslt libxslt-devel libxml2 libxml2-devel gd gd-devel GeoIP GeoIP-devel -y`


Tải Nginx và giải nén

`yum install -y wget && wget https://nginx.org/download/nginx-1.9.9.tar.gz && tar xzvf nginx-1.9.9.tar.gz`

Di chuyển vào thư mục được trích xuất để biên dịch Nginx từ nguồn. Liệt các tập tin như sau:

```
[U@sv ~]$ cd nginx-1.9.9
[U@sv nginx-1.9.9]$ ls -alh
total 652K
drwxr-xr-x. 8 1001 1001  158 Dec  9  2015 .
dr-xr-x---. 3 root root  180 May 20 21:30 ..
drwxr-xr-x. 6 1001 1001 4.0K May 20 21:30 auto
-rw-r--r--. 1 1001 1001 251K Dec  9  2015 CHANGES
-rw-r--r--. 1 1001 1001 382K Dec  9  2015 CHANGES.ru
drwxr-xr-x. 2 1001 1001  168 May 20 21:30 conf
-rwxr-xr-x. 1 1001 1001 2.5K Dec  9  2015 configure
drwxr-xr-x. 4 1001 1001   72 May 20 21:30 contrib
drwxr-xr-x. 2 1001 1001   40 May 20 21:30 html
-rw-r--r--. 1 1001 1001 1.4K Dec  9  2015 LICENSE
drwxr-xr-x. 2 1001 1001   21 May 20 21:30 man
-rw-r--r--. 1 1001 1001   49 Dec  9  2015 README
drwxr-xr-x. 9 1001 1001   91 May 20 21:30 src
```


Để xem tất cả các tùy chọn, cần chạy lệnh script bên dưới và kiểm tra các tùy chọn nên được cấu hình để cài đặt Nginx. Hoặc bạn cũng có thể cài đặt cấu hình mặc định bằng cách chạy lệnh cònigure script mà không có bất kỳ tùy chọn nào theo sau nó.

`./configure --help`

Chạy các lệnh Configure script theo sau là các tùy chọn cần thiết:

`./configure --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --error-log-path=/var/log/nginx/error.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --user=nginx --group=nginx`

Thực hiện lệnh `make` cho quá trình cấu hình.

`make`

Cài đặt Nginx với lệnh make Install.

`make install`

Kiểm tra Nginx xem đã cài đặt thành công hay chưa.

`nginx -v`

Tạo người dùng cho Nginx và đặt quyền sở hữu thích hợp cho thư mục cài đặt Nginx bằng cách chạy lệnh sau.

```
useradd nginx
chown -R nginx:nginx /etc/nginx/
```

Cấu hình dịch vụ cho Nginx bằng cách sử dụng trình chỉnh sửa vi và nhập nội dung sau vào tệp. Lưu và thoát tệp.


`vi /usr/lib/systemd/system/nginx.service`

Thêm nội dung sau vào tệp:

```
[Unit]
Description=nginx - high performance web server
Documentation=https://nginx.org/en/docs/
After=network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/var/run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t -c /etc/nginx/conf/nginx.conf
ExecStart=/usr/sbin/nginx -c /etc/nginx/conf/nginx.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s TERM $MAINPID

[Install]
WantedBy=multi-user.target
```

Start và Enable Dịch vụ Nginx bằng cách chạy lệnh sau:

```
systemctl start nginx
systemctl enable nginx
```

Chuyển sang trình duyệt và nhập IP của máy. Trang web mặc định của Nginx được hiển thị trong trình duyệt như bên dưới.

![Imgur](https://i.imgur.com/aJVbSMY.png)


Link tham khảo: https://www.linuxhelp.com/how-to-install-nginx-from-source-code-on-centos-7