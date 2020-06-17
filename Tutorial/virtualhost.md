# Tạo Virtualhost trong Nginx

Virtual host là kỹ thuật cho phép nhiều website có thể dùng chung tài nguyên một IP. 

Sau đây sẽ là bài viết cấu hình virtualhost cho Ngĩn và cấu hình php-fpm để virtualhost xử lý được file php.

## Mô hình cài đặt 
## Cài đặt php
Chạy và cài các lệnh liên quan đến php.

`yum install -y php-common php-bcmath php-cli php-devel php-mcrypt php-mysql php-password-compat php-pclzip php-pdo php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-dba php-embedded php-enchant php-mbstring php-intl libssh2 php-pecl-ssh2 php-pecl-memcached php-pecl-memcache php-fpm`

Sửa file cấu hình của php-fpm

