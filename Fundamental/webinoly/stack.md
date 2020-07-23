
## Command Stack

Lệnh Stack trên nền tảng cho phép chúng ta cài đặt và gỡ bỏ các gói hoặc công cụ được cài đặt bởi webinoly.

Cú pháp :

`stack <option> <option2>`

Option: 
* -html
* -lemp
* -mysql
* -php
* -php-ver
* -pma
* -purge
* -purge-server-all

### cài đặt 
* nginx
  * `stack nginx`
* php
  * `stack -php`
* MySQL(MariaDB)
  * `stack -mysql` 
* Install PHPMyAdmin: Được cài đặt tự động với MySQL
  * `stack -pma`
* Install LEMP: Cài đặt Nginx, PHP, MySQL và các công cụ bổ sung.
  * `stack -lemp`

### Thay đổi hoặc sửa đổi phiên bản PHP

`stack -php-ver=7.2`

### Xóa các gói

`stack [package] -purge`

Package:
* Nginx
* PHP
* mysql 
* pma

#### Xóa tất cả các gói được cài đặt bởi Webinoly

`stack -purge-server-all`

hoặc 

`-purge-server-all=force`

#### Xóa và thu hồi tát cả các Chứng chỉ SSl

>**Khuyến nghị**: Nên xóa và thu hồi trước khi xóa Nginx:

`site -delete-all -revoke=on`

`site domain.com -delete=force -revoke=on`

Chọn website chỉ định

`site domain.com -ssl=off -revoke=on`


Nếu hệ thống sẽ hỏi lại bạn có thực sự muốn xóa không.
Nhập **y** để đồng ý và **N** để hủy bỏ.