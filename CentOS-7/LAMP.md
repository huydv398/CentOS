# LAMP in CentOS-7
LAMP là một hệ thống các phần mềm để tạo dựng môi trường máy chủ web được viết bằng PHP. Có khả năng chứa và phân phối các trang web động được viết bằng PHP.

LAMP gồm có:
* **Linux**: Là hệ điều hành, cũng là phần mềm dùng để điều phối và quản lý các tài nguyên của hệ thống.
* **Apache**: Là phần mềm máy chủ web, có thể thực hiện các request(Yêu cầu) được gọi tới máy chủ thông qua các giao thức HTTP.
* **Mysql/Mariadb**: Là hệ quản trị cở sở dữ liệu giúp lưu trữ và truy xuất dữ liệu.
* **PHP**: là ngôn ngữ lập trình cho kịch bản hoạt động của máy chủ.

## Tiến hành cài đặt
### 1.Cài đặt Linux
Là cài đặt hệ điều hành hay môi trường để các phần mềm hoạt động. Linux có nhiều bản phân phối như Debian, RedHat,Ubuntu,... Trong bài viết này sử dụng CentOS-7.
### 2.Cài đặt Apache 
Để cài đặt, trên cửa sổ terminal gõ lệnh:

`yum install -y httpd`

Cài xong, tiến hành khởi động service:

` systemctl start httpd`

` systemctl enable httpd`

Kiểm tra trạng thái hoạt động trên server:

`systemctl status httpd`

Kiểm tra trạng thái hoạt động trên web:

`[Địa chỉ IP Server]`

Nếu sử dụng hệ điều hành trên máy ảo, tắt firewall để có thể truy cập trên browser máy thực:

`systemctl stop firewalld`

### 3.Cài hệ quản trị cơ sở dữ liệu
Trên thực tế với LAMP, bạn có thể sử dụng MySQL hoặc Mariadb.

Trên cửa sổ Teerminal, tiến hành cài Mariadb:

`yum -y install mariadb mariadb-server `

Tiến hành khởi động mariadb service: 

`systemctl start mariadb `

Cài lại mật khẩu cho root của cơ sở dữ liệu:

`mysql_secure_installation`

Thiết lập một số cấu hình tại bước này như sau:

### 4.Cài đặt PHP
Cài đặt phiên bản mới nhất.Tiến hành thêm kho Remi CentOS:

`yum install epel-release`

`yum update epel-release`

`rpm -Uvh https://rpms.remirepo.net/enterprise/remi-release-7.rpm`

Cài Yum-utils vì cần tiện ích yum-config-manager để cài đặt:

`yum -y install yum-utils`

Tiến hành cài đặt PHP. Ở đây ta cần lưu ý về phiên bản cài đặt như sau:

* Ver 7.0:
`yum-config-manager --enable remi-php70`
* Ver 7.1:
`yum-config-manager --enable remi-php71`
* Ver 7.2:
`yum-config-manager --enable remi-php72`
* Ver 7.3:
`yum-config-manager --enable remi-php73`

Cài đặt các Option php
`yum -y install php php-opcache php-mysql`

Tiến hành kiểm tra kết quả. Ta thêm file sau:

`echo "<?php phpinfo(); ?>" > /var/www/html/info.php`

Sau khi cài đặt , thực hiện restart lại Apache:

` systemctl restart httpd`

Vào trình duyệt, gỗ trên thanh URL địa chỉ như sau:

`[Địa chỉ IP]/info.php`

Sau khi màn hình xuất hiện, đã thực hiện thành công!

