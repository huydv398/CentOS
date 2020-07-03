# WordOps
Là một bộ công cụ giúp giảm bớt quản trị trang web và máy chủ WordPress 

WordOps v3.12.2

WordOps cung cấp khả năng triển khai WordPress nhanh và bảo mật với Nginx bằng cách sử dụng các lệnh đơn giản và dễ nhớ. Được bổ sung từ EasyEngine v3(là một script cho hệ điều hành Ubuntu/Debian để tự cài đặt NGINX Webserver dành riêng cho WordPress, có phpMyAdmin, PHP-FPM và nhiều tính năng độc đáo khác).

Công cụ được xây dựng bằng Python 3

Vị trí file cấu hình- file-WordOps

* **/etc/wo/wo.conf**

## Các tính năng chính
* Dễ dàng cài đặt: Trình cài đặt tự động
* Triển khai nhanh: Cài đặt WordPress, Nginx, PHP, MySQL & Redis nhanh và tự động.
* Tùy chỉnh Nginx Build
* Cập nhật: PHP 7.2, 7.3, 7.4; MariaDB 10.3 & Redis 5.0
* Bảo mật: Bảo mật WordPress tốt hơn với các locations directives (các chỉ thị location)
* Mạnh mẽ: Cấu hình Nginx được tối ưu hóa với nhiều multiple cache backends support (hỗ trợ phụ trợ bộ nhớ đệm)
* SSL: Domain, SubDomain & Wildcard Let's Encrypt SSL certificates(chứng chỉ) với DNS API
* Giám sát: Lưu lượng truy cập vhost Nginx trực tiếp với ngx_vts_module và giám sát máy chủ với Netdata
* Thân thiện với người dùng: Bảng điều khiển WordOps với các công cụ / giám sát và trạng thái máy chủ.

## Usage Command

Command: 

* `wo [OPTIONS] {ARGUMENTS}`

OPTIONS:
* `--version`: Hiển thị phiên bản đang sử dụng
* `info`: Hiện thị các thông tin như Nginx, PHP, MySQL, 
* **log** : `wo log (gzip | reset | show) [tên domain] {ARGUMENTS}`
    * `gzip`: Nén file nhật ký
    * `reset`: reset (xóa nhật ký tệp)
    * `show`: hiển thị các file nhật ký
    * **{ARGUMENTS}**:
        * `--all`          Reset All logs file
        * `--nginx`        Reset Nginx Error logs file
        * `--php`          Reset PHP Error logs file
        * `--fpm`          Reset PHP-FPM slow logs file
        * `--mysql`        Reset MySQL logs file
        * `--wp`           Reset Site specific WordPress logs file
        * `--access`       Reset Nginx access log file

Ví dụ 
* `wo log gzip haha.duonghuy.xyz --all`: 
    * Câu lệnh dùng để nén tất cả file nhật ký của website haha.duonghuy.xyz
* `wo log show haha.duonghuy.xyz --access`
    * Câu lệnh dùng hiện thị các file access.log của website haha.duonghuy.xyz


* stack : 
    * `install` : Cài đặt tất cả các gói phụ trợ WordOps hoặc cài từng phần riêng biệt bằng cách thêm các gói phụ trợ
        * `--mysql`
        * `--nginx`
        * `--php`
        * `--phpmyadmin`
        * **Command**: `wo stack install --nginx --php`
    * `remove`: xóa tất cả gói phụ trợ hoặc xóa các gói không cần thiết bằng cách thêm gói phụ trợ 
    * `purge`: Gỡ cài đặt và lọc bỏ tất cả các gói hoặc các gói chỉ định
    * `upgrade`: Cập nhật tất cả các gói hoặc các gói chỉ định 
    * `status`: Trạng thái tất cả các gói hoặc các gói chỉ định
    * `stop`: Dừng dịch vụ tất cả các gói hoặc các gói chỉ định
    * `reload`: reload tất cả các gói hoặc các gói chỉ định
    * `restart`: restart tất cả các gói hoặc các gói chỉ định
* `site` :
    * `cd [domain]`: cd đến thư mục của web 
    * `list --enabled/--disabled`: Liệt kê danh sách các site đang được kích hoạt/ chưa được kích hoạt
    * `info [domain]` Thông tin domain, user/pass
    * `show`: Thông tin các file lưu cấu hình và các cấu hình gọi vào site
    * `enable`/`disable` kích hoạt hoặc tạm ngưng dịch vụ của trang web
    * `edit`: Sửa các file cấu hình của site muốn chọn
    
* `site create/update example.com`: Tạo hoặc update một site
    * `--html`: Tạo hoặc Update site --html
    * `--php`:
    * `--php73`:
    * `--mysql`: Tạo một website PHP+MySQL 
    * `--wpfc`: Tạo trang web WordPress với Nginx fastcgi_cache
* `site delete example.com`
    * `--db`: xóa database của trang web
    * `--files`: xóa trang web chính.
    * `--no-prompt`: Không cần xác nhận khi xóa
* secure [--auth | --port | --ip |--ssh | --sshport]
    * `--auth`: Cập nhật thông tin xác thực của HTTP
    * `--port`: Thay đổi port WordOps Admin hiện tại là 22222.
    * `--ip`: 
    * `--ssh`: Tăng cường bảo mật ssh. Tạo khóa SSH RSA
    * `--sshport` thay đổi port ssh mặc định là 22

### Usage 

## Yêu cầu phần cứng 

Tài nguyên tối thiểu

WordOps rất nhẹ, nó không đòi hỏi quá nhiều tài nguyên và có thể được cài đặt trên các thiết bị cấp thấp như Raspberry PI(máy tính chỉ bằng thẻ tín dụng). Yêu cầu tối thiểu là:
* Bộ nhớ có dung lượng lưu trữ 100MB
* RAM 512MB

Cấu hình đề nghị:
* CPU đa lõi
* Bộ nhớ SSD 20GB
* RAM 2GB

### Vitualization
Các nền tảng ảo hóa được hỗ trợ:
* VMware
* OpenVZ
* KVM
* Hyper-V

WordOps cũng tương thích với Ubuntu chạy trên Hệ điều hành Linux

Khuyến nghị cấu hình máy chủ
* Đặt tên máy hợp lệ

Cấu hình tên máy chủ phù hợp:

Tên máy chủ của máy chủ không chỉ là một tên, đó là danh tính công khai của máy chủ trên mạng. Nếu máy chủ của bạn được kết nối trực tiếp với Internet (không phải sau NAT), thì máy chủ đó phải có tên máy chủ hợp lệ.

Một tên máy chủ hợp lệ: myservername.yourdomain.com
* myservername là tên máy chủ
* yourdomain.com là một trong những tên miền của bạn

Chỉnh sửa tên máy chủ :

`hostnamectl set-hostname [yourserver.hostname.com]`

Để áp dụng máy chủ mới, cần khởi động lại. Bước cuối cùng và quan trọng nhất, bạn nên tạo bản ghi DNS thích hợp để tạo subdomain trỏ đến IP máy chủ của bạn.

## Cài đặt 
### Tải về các phụ thuộc
Để làm việc với WordOps rất đơn giản vì đã được cung cấp 1 tập lệnh cài đặt để cài đặt các phụ thuộc cần thiết, chạy câu lệnh sau để cài đặt:

`wget -qO wo wops.cc && sudo bash wo`

![Imgur](https://i.imgur.com/TRXdxev.png)

* 1: Điền tên của bạn
* 2: Điền địa chỉ Email của bạn

### Kích hoạt bash_completion
Để bật tự động hoàn thành các lệnh WordOps, hãy chạy lệnh sau sau khi cài WordOps:

`source /etc/bash_completion.d/wo_auto.rc`

### Cài đặt WordOps Stack

Bạn có thể lựa chọn danh sách các thành phần WordOps (Nginx, php, mysql, ...) hoặc cài và đến khi thiết lập trang web cần các phụ thuộc nào thì WordOps sẽ tự động cài thành phần đó.

Với câu lệnh sau WordOps sẽ cài tát cả các thành phần phụ thuộc:

`wo stack install`

Chờ 2-3 phút để WordOps thực hiện cài đặt các thành phần:

![Imgur](https://i.imgur.com/E963HWJ.png)

Phần cuối là User và Pass để đăng nhập HTTP Auth
 
 ![Imgur](https://i.imgur.com/wcfu9cw.png)

### Đặt lại tài khoản và mật khẩu khi đăng nhập HTTP Auth

`wo secure --auth`

![Imgur](https://i.imgur.com/Nw51i1Y.png)

* Nhập Username mới hoặc Enter để sử dụng đề xuất User của WordOps.
* Nhập password mới hoặc Enter để sử dụng đề xuất Pass của WordOps.

Sau khi nhập xong, nếu muốn truy cập web monitoring, chuyển đến trình duyệt và điều hướng đến link sau: `https://địa_chỉ_ip_server:22222`

## Tạo trang web
### Tạo trang web với WordPress

Để tạo một trang WordPress, bạn cần chạy câu lệnh sau:

`wo site create wp.duonghuy.xyz --wp`

Sau khi chạy lệnh trên, WordOps sẽ tạo 1 trang WordPress với Domain là **wp.duonghuy.xyz**(thay tên Domain bằng tên Domain của bạn). Sau khi lệnh cài đặt xong sẽ có tài khoản và mật khẩu sử dụng để truy cập trang quản trị của WordPress:

![Imgur](https://i.imgur.com/LQFOGkc.png)

Và WordOps sẽ tạo ra User và Pass để đăng nhập quản trị WordPress

Truy cập vào Url để kiểm tra tên miền

![Imgur](https://i.imgur.com/QyCafPs.png)

Truy cập vào trang quản trị:

**Domain/wp-admin** - wp.duonghuy.xyz/wp-admin

![Imgur](https://i.imgur.com/fWevoFg.png)


User và Pass được WordOps cung cấp phần cuối dòng lệnh.

![Imgur](https://i.imgur.com/l49cIdC.png)

#### Tạo một trang web khác

Nếu không cài WordPress, bạn cũng có thể sử dụng các tùy chọn của WordOps để cài các trang web cơ bản như:

* Tạo trang **Web HTML**

` wo site create html.duonghuy.xyz --html`

Kiểm tra thông tin:

`wo site info html.duonghuy.xyz`

![Imgur](https://i.imgur.com/vRn6nDA.png)

Thêm nội dung vào trang web

`echo 'html.duonghuy.xyz' >> /var/www/html.duonghuy.xyz/htdocs/index.html`

Kiểm tra:

![Imgur](https://i.imgur.com/cxS1pNm.png)

* Tạo trang **Web PHP**

`wo site create php.duonghuy.xyz --php`

* Tạo trang Web PHP + MySQL

`wo site create web.duonghuy.xyz --mysql`

### Cập nhật thông tin trang web

Xem thông tin trang web:

Để biết các tùy chọn khi sử dụng:

`wo site -h` 

hoặc 

`wo site --help`


Để liệt kê các Web site được tạo và quản lý bởi WordOps:

`wo site list`

#### Để xem thông tin chi tiết của 1 web site, ta sử dụng:

`wo site info wp.duonghuy.xyz`

![Imgur](https://i.imgur.com/HZwOQAl.png)

#### Cập nhật trang web

Nếu trước đó bạn đã tạo một trang WordPress với WordOps mà chưa có Let's Encrypt, bạn có thể sử dụng lệnh sau để cập nhật chứng chỉ SSL cho site như sau:

`wo site update wp.duonghuy.xyz -le`

Sau khi tạo SSL sẽ có thời gian 80 ngày, nhưng tất cả các chứng chỉ được tự động gia hạn 60 ngày bởi `acme.sh` bằng cách sử dụng Cronjob.

#### Vô hiệu hóa tạm thời trang web và kích hoạt

Khi không có nhu cầu sử dụng trang web, ta có thể vô hiệu hóa trang web như sau:

`wo site disable html.duonghuy.xyz`

Khi muốn sử dụng lại website

`wo site enable html.duonghuy.xyz`

#### Xóa 1 trang Web 

Sử dụng tùy chọn `delete`

`wo site delete php.duonghuy.xyz`

WordOps yêu cầu xác nhận **Y** để xác nhận xóa:

```
Deleting Webroot, /var/www/php.duonghuy.xyz
Deleted webroot successfully
Testing Nginx configuration     [OK]
Reloading Nginx                 [OK]
Deleted site php.duonghuy.xyz


```


## Truy cập trang Web Monitoring

Sau khi cài đặt trang web, để theo dõi tải cũng như hiệu năng của trang web ta có thể truy cập theo địa chỉ https//IP.server:22222 để theo dõi tải cũng như các thông số giám sát của web.

![Imgur](https://i.imgur.com/wtLpjNi.png)

Mặc định Port để đăng nhập WordOps là 22222 muốn thay đổi bằng câu lệnh WordOps ta làm như sau:

`wo secure --port`

Nhập port muốn thay đổi: Ở đây tôi đổi thành port 51234

![Imgur](https://i.imgur.com/QIKcxfc.png)

Và đăng nhập lại:

https://IP_server:51234

![Imgur](https://i.imgur.com/cpaq4G6.png)

### `--sshport` thay đổi port ssh mặc định là 22

Mặc định port ssh là 22 sử dụng WordOps để thay đổi port SSH:

`wo secure --sshport`

Nhập port mới:

![Imgur](https://i.imgur.com/LLnmklJ.png)

Kiểm tra lại

SSH bằng Port 22

![Imgur](https://i.imgur.com/YLj7rJq.png)

SSH bằng port mới:

![Imgur](https://i.imgur.com/b1qxlVg.png)

![Imgur](https://i.imgur.com/DY21gmw.png)

![Imgur](https://i.imgur.com/uBvFwIA.png)

Đổi port SSH thành công.

## Kết luận.
WordOps là một công cụ sử dụng dòng lệnh để cài đặt nhanh chóng các Website chỉ bằng 1 dòng lệnh

Câu lệnh dễ làm việc

Xây dựng bằng câu lệnh Python3

* Hiện thị, nén, xóa log.
* Cập nhật, cài đặt, status, enable/disable, restart/reload, remove tất cả các gói phụ trợ mà WordOps hỗ trợ hoặc các thể làm việc với từng gói phụ trợ tên lẻ như mysql, nginx, php, phpmyadmin,...
* Tạo, cập nhật, xóa, hiển thị danh sách, thông tin, enable/disable, chỉnh sửa từng site riêng biệt.
* Tùy chỉnh tăng cơ chế bảo mật như:
    * Thay đổi thông tin xác thực HTTP
    * Thay đổi port WordOps Admin 
    * Tạo khóa SSH RSA
    * Thay đổi Port SSH

