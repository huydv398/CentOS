# WordOps
Là một bộ công cụ giúp giảm bớt quản trị trang web và máy chủ WordPress 

WordOps v3.12.2

WordOps cung cấp khả năng triển khai WordPress nhanh và bảo mật với Nginx bằng cách sử dụng các lệnh đơn giản và dễ nhớ. Được bổ sung từ EasyEngine v3(là một script cho hệ điều hành Ubuntu/Debian để tự cài đặt NGINX Webserver dành riêng cho WordPress, có phpMyAdmin, PHP-FPM và nhiều tính năng độc đáo khác).

Công cụ được xây dựng bằng Python 3

Vị trí file cấu hình- file-WordOps

**/etc/wo/wo.conf**

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

`wo [OPTIONS] {ARGUMENTS}`

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
    * `info [domain]` Thông tin domain
    * `show`
    * `enable`/`disable`
    * `edit`
    
    * show
* site create/update example.com
    * --html
    * --php
    * --php73
    * --mysql 
* site delete example.com
    * --db
    * --files
    * --no-prompt
* secure [--auth | --port | --ip |--ssh | --sshport]


### Usege 

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

