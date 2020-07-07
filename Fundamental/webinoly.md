# Webinoly

Webinoly cung cấp một bộ công cụ rất tiện lợi giúp việc tạo và quản trị website. Với Webinoly việc quản trị website trở nên dễ dàng hơn rất nhiều. Chỉ bằng một lệnh bạn đã có 1 trang web hay có thể tạo được SSL cho trang web của bạn. Webinoly mới chỉ hỗ trợ cho hệ điều hành là Ubuntu 16.04 và Ubuntu 18.04

Webinoly tuân theo các thực tiễn tốt nhất cho trang web của bạn:
* Chứng chỉ SSL miễn phí cho trang web của bạn với Lets Enscrypt 
* HTTP /2 tăng đáng kể tốc độ phục vụ nội dung của bạn
* PHP v7.4 và hỗ trợ cho các phiên bản trước nếu cần.
* FastCgi Cache và Redis Object Cache cho các trang web WordPress của bạn.
* Tự động hóa máy chủ của bạn để tận dụng tối đa các tài nguyên có sẵn.

Quản Lý trang web dễ dàng
* Các lệnh duy nhất để tạo, xóa , vô hiệu hóa các trang web.
* Hỗ trợ cho mọi loại trang web HTML, PHP, WordPress hoặc bất kỳ cấu hình nào theo sở thích của bạn trong môi trường LEMP
* Cài đặt chứng chỉ SSK với cấu hình tự động.
* Trình quản lý chuyển hướng Nginx, Sao lưu, SMTP và nhiều tính năng khác.
* Tích hợp dữ liệu gốc để theo dõi và phân tích.
* Sửa đổi cấu hình máy chủ của bạn bất cứ lúc nào theo nhu cầu của bạn.
* Lịch sử người xem trong thời gian thực

1 Click ! WordPress 
* Lệnh tạo và định cấu hình trang web WordPress, thậm chí là nhiều trang đa chức năng trong tên miền phụ hoặc thư mục con.
* Cấu hình tự động wp-config.php và cơ sở dữ liệu để bạn có thể bắt đầu sử dụng trang web của mình càng sớm càng tốt mà không cần chỉnh sửa dòng lệnh nào.
* Kích hoạt thêm một lớp bảo vệ trên trang đăng nhập WordPress bằng xác thực HTTP. 
* Cài đặt WordPress trong bất kỳ thư mục con hoặc Clone một trang WordPress để làm cho quá trình phát triển dễ dàng hơn.
* Công nghệ cache tiên tiến cho trang web WordPress của bạn với FastCgi và Redis.

>**Lưu ý**: Webinoly giúp bạn dễ dàng tự động hóa các cài đặt của mình vì nó được thiết kế để hoạt động và được đưa vo tập lệnh của bạn theo cách hoàn toàn không giám sát, cho nhu cầu phục hồi nhanh
## Cấu hình đề nghị
Để cài đặt và quản lý website sử dụng server có cấu hình tối thiểu như sau:
* Hệ điều hành: Ubuntu 18.04
* CPU: 1core
* DISK: 10GB
* Network: 1 Interface(trong bài sử dụng địa chỉ IP có địa chỉ 103.101.161.199)

### Cài đặt

Sử dụng câu lệnh:

`wget -qO weby qrok.es/wy && sudo bash weby 3`

Việc cài đặt sẽ mất khoảng 5-7 phút. Sau khi cài đặt xong, kết quả

![Imgur](https://i.imgur.com/5QM74Ke.png)


Khi cài đặt hoàn tất sẽ có 2 thư mục được xuất hiện là **sites-available** và **www**:
* **sites-available** là thư mục **/etc/nginx/sites-available**: là thư mục chứa file cấu hình của trang web
* **www** là thư mục **/var/www/**: là webroot của web site chứa các file hiển thị như **.html**, **.php**, **.image**, ...

## Command HTTPAUTH
Xác thực HTTP cơ bản
* Tạo user 
* Xóa user 
* list
* Wordpress login
* Bảo vệ thư mục tùy chỉnh hoặc file

Lệnh của HTTP Auth, cho phép chúng tôi quản lý người dùng có quyền truy cập các trang được bảo vệ bởi phương thức xác thực HTTP, Ngoài việc kiểm soát kích hoạt lớp bảo mật bổ sung này trong các tang truy cập cung cụ như PHPMyAdmin và wp-admin hoặc wp-login. Về cơ bản, nó là để bảo vệ một số thành phần trong trang web của bạn yêu cầu người dùng và mật khẩu để có thể truy cập nội dung của nó.

Cú pháp:

`sudo httpauth <option>`

Option:
* -add :
* -delete
* -list
* -path
* -whitelist
* -wp-admin

### Tạo user 

Tạo người dùng và pass cho phép truy cập vào các phần được bảo vệ bằng Xác thực HTTP, hãy sử dụng lệnh:

`httpauth -add`

Bạn cũng có thể tạo người dùng có quyền hạn chế để chỉ truy cập một tên miền cụ thể.

`httpauth example.com -add`

Sau khi bạn đã tạo một hoặc nhiều người dùng cho một tên miền cụ thể **Chỉ những người dùng này được phép truy cập tên miền này**

Ví dụ:

Tạo một trang Wordpress

`site huy.vn -wp `

Truy cập bằng trình duyệt:

![Imgur](https://i.imgur.com/6rxn6hg.png)

Trình duyệt yêu cầu xác thực 

### Xóa User 
Để xóa người dùng sử dụng lệnh sau:

`httpauth -delete`

Xóa User từ một tên miền cụ thể

`httpauth example.com -delete`

### Danh sách
Hiển thị danh sách tất cả người dùng được tạo với quyền truy cập vào HTTP Auth

`httpauth -list`

Hiện thị User của tragn web 

`httpauth example.com -list`

### wp-admin

Lớp bảo mật này có thể gây khó chịu cho một số người dung, nếu bạn bật/tắt HTTPAuth trong các trang đăng nhập Wordpress, và trang web sau đó sẽ mất thiết lập này.

`httpauth -wp-admin=off`

`httpauth example.com -wp-admin=off`

Bảo vệ tùy chọn folder hoặc file

`httpauth example.com -path=/folder`

### Whitelist IP
Để thêm IP vào Whitelist và không được yêu cầu thông tin đăng nhập mỗi khi yêu cầu xác thực HTTP
`httpauth -whitelist`

Xóa địa chỉ IP khỏi danh sách whitelist IP

`httpauth -whitelist -purge`
## Command **site**
* Dùng để tạo bất kỳ một trang web mới của bạn, thêm chứng chỉ SSL, kích hoạt FastCgi Cache cho cài đặt WordPress của bạn và nhiều tính năng khác sẽ cho phép bạn có toàn quyền kiểm soát các trang web của mình một cách dễ dàng và đáng tin cậy, luôn luôn ở một lệnh 

`sudo site <domain> <option> <option2>`

Option:
* -cache : bật tắt chế độ cache
* -clone-from : sao chép từ một trang web wp khác
* -delete : xóa bỏ
* -delete-all: Xóa hết
* -force-redirect=(Option)
    * www:
    * root
    * off
* -forward: chuyến tiếp chuyển hướng trang web
* –html: Tạo trang web HTML
* –php : tạo trang PHP  
* -wp : Tạo một website Wordpress 
* –mysql: Tạo một trang web php kết hợp với cơ sở dữ liệu
* -list: Hiển thị các trang web đã tạo
* -multisite-convert
* -parked: thay thế bổ sung trỏ đến trang web chính
* -proxy: 
* -redirection: định hướng
* –ssl: Chứng chỉ bảo mật 
### Tạo một trang web mới
Tạo một trang web mới thuộc bất kỳ loại nào: HTML, PHP, WordPress, Reverse Proxy, v.v.

Tạo một trang HTML cơ bản:

`site demo.com -html`

Ví dụ:
```
~# site html.dh.vn -html

~# echo "Website's HTML" > www/html.dh.vn/htdocs/index.html
```

![Imgur](https://i.imgur.com/ZRugttj.png)

Tạo một trang web có hỗ trợ PHP

`site demo.com -php`

Demo

```
~# site php.dh.vn -php
echo "<?php phpinfo(); ?>" > www/php.dh.vn/htdocs/index.php
```

![Imgur](https://i.imgur.com/LzZLfut.png)

Tạo một trang web PHP kết hợp với cơ sở dữ liệu:

`site demo.com -mysql`

Demo
```
~# site sql.dh.vn -mysql

Site sql.dh.vn has been successfully created!


Database Host: localhost
Database Name: sql_dh_vn
Database User: sql_dh_vn
Password: 6nSETl0yqURN93P5

Database successfully created!
```
Tùy chọn chỉnh sửa web trước khi tạo:

`site web.duonghuy.xyz  -mysql=custom`

Câu lệnh trên dùng để tạo một trang web và người tạo phải truyền vào dữ liệu của MySQL

### Tạo 1 trang WordPress
```
site domain -wp
#lệnh sau để tắt login admin khi truy cập
httpauth domain -wp-admin=off
```

![Imgur](https://i.imgur.com/g3tVIzb.png)

![Imgur](https://i.imgur.com/xvLGf4i.png)

![Imgur](https://i.imgur.com/QMfLd4B.png)

![Imgur](https://i.imgur.com/9DpR3Of.png)

![Imgur](https://i.imgur.com/oAdCcrk.png)
 

### WordPress Multisite
Chuyển đổi trang WordPress sang [WordPress Multisite](Note/multisite.md)

`site example.com -multisite-convert`

Có 2 lựa chọn để quản lý là **Subdomain** và **Subfolder**

### Bộ nhớ cache FastCGI

Cùng với Nginx tối ưu hóa để tăng tốc trang web WordPress của mình. 

Để bật FastCGI:

`sudo site example.com -cache=on`

Để tắt FastCGI:

`sudo site example.com -cache=off`

Cũng có thể kích hoạt nó từ việc tạo trang mới

`sudo site example.com -wp -cache=on`

### Cấu hình thư mục con
Khả năng định cấu hình độc lập từng thư mục con của một trang web cụ thể.

```
sudo site example.com -html -subfolder=/one
sudo site example.com -php -subfolder=/two
sudo site example.com -wp -subfolder=/three
sudo site example.com -proxy -subfolder=/four

# Delete a subfolder site
sudo site example.com -delete -subfolder=/xxx
```

Là một tùy chon để có một trang web ở gốc của tên miền.
Nó có thể hỗ trợ các thư mục con được cấu hình trong bất kỳ kết hợp cấu hình nào bạn cần

```
# Example with subfolders and empty root
+ example.com <empty>
  - /blog <wp>
  - /downloads <proxy>
  - /tickets <php>

# Another example with static site in root
+ example.com <html>
  - /news <wp>
  - /clients <php>
```
### Tên miền chưa được sử dụng hoặc bí danh
Một tên miền chưa được sử dụng là một tên miền bổ sung hoặc thay thế trỏ đến trang web chính. Đó là một cách đơn giản để truy cập trang web của bạn từ các tên miền khác nhau.

`sudo site example.com -parked`

```
~# site web1.dh.vn -parked

Site web1.dh.vn has been successfully created!
#Nhập tên miền muốn chuyển tiếp đến
Main site domain: wp4.dh.vn

Parked domain was successfully configured!
```
Khi thực hiện lệnh, bạn sẽ được yêu cầu nhập tên miền chính nơi tên miền chưa được sử dụng mới sẽ trỏ đến. Theo cùng một cách bạn có thể sử dụng lệnh sau để tạo điều kiện cho việc sử dụng nó:

`sudo site example.com -parked=mainsite.com`

Đảm bảo trang web chính được lưu trữ trên cùng một máy chủ.

### Chuyển tiếp tên miền 

Tất cả các yêu cầu cho tên miền này sẽ được chuyển hướng hoặc chuyển tiếp đến tên miền khác.

`sudo site example.com -forward`

```
~# site wp.dh.com -forward

#Nhập tên miền muốn chuyển tiếp
Destination domain: wp.dh.vn
Site wp.dh.com has been successfully created!
Every request to wp.dh.com will be redirected to http://wp.dh.vn
```
Tất cả các tham số yêu cầu được chuyển đến tên miền mới. Nếu bạn muốn xóa các tham số này và buộc chuyển hướng đến một trang web duy nhất, Bạn có thể sử dụng tùy chọn `-root=on`


### Reverse Proxy site

Để tạo một trang web có cấu hình Reverse Proxy trong Nginx:

`sudo site example.com -proxy=[localhost:8080]`



### Tạm thời vô hiệu hóa một trang web

Bất cứ lúc nào cũng có thể kích hoạt hoặc tạm ngưng một trang web được lưu trữ trên máy chủ của bạn mà không xóa nó

`sudo site example.com -on`

hoặc

`sudo site example.com -off`

#### Xóa một trang web
Sử dụng câu lệnh một cách cẩn thận, vì khi đã xóa một trang web, nó sẽ không thể khôi phục các tệp dữ liệu

`sudo site example.com -delete`

Để xóa tất cả các trang web trên máy chủ của bạn:

`site -delete-all`

Khi một trang web bị xóa, nếu tìm thấy CSDL bên ngoài, một nỗ lực sẽ được thực hiện để sử dụng dữ liệu `-external-db`, nếu chúng không được tìm thấy, người dùng sẽ được yêu cầu nhập dữ liệu cần thiết. `-revoke=on` Tham số này sẽ xóa và thu hồi Chứng chỉ SSL của một trang web nếu được tìm thấy, hãy sử dụng tùy chọn `off` để giữ chứng chỉ SSL.

#### Danh sách các trang web
Để xem danh sách tất cả các trang web của bạn được lưu trữ trên máy chủ, hãy sử dụng lệnh sau:

`site -list`


### Chứng Chỉ SSL với Let's-Encrypt 

`site domail.com --ssl=on`

* Trong quá trình tạo chứng chỉ, Webinoly sẽ gửi yêu cầu tài khoản Email của bạn, Email này sẽ được sử dụng để đăng ký chứng chỉ mới, ngoài ra còn giúp bạn theo dõi quá trình gia hạn định kỳ.

* Các chứng chỉ do Let Encrypt cấp có giá trị trong khoảng thời gian 90 ngày. Webinoly tự động kiểm tra mỗi tuần một lần trạng thái của các chứng chỉ. Webinoly tự động hoàn tất quy trình để giữ cho chứng chỉ của bạn và các trang web của bạn luôn có hiệu lực.


* Vô hiệu hóa SSl trên một trang web
Bận hủy kích hoạt việc sử dụng SSL trong trang web, thực hiện lệnh sau:

`site example.com -ssl=off`

Các tùy chọn `ssl=off` có thể được sử dụng nhay cả khi tran gweb của bạn thậm chí không tồn tại nữa như là một cách để loại bỏ và thu hồi SSL.
### Chứng chỉ trên các trang web packed
Tùy chọn `-root` cho phép bạn tạo chứng chỉ cho các trang web nơi gốc của các tệp của bạn ở một nơi khác so với thông thường
### Chứng chỉ trên các trang web Reverse Proxy
Tùy chọn `-root-path` cho phép chúng tôi chỉ định một tuyến đường đi khác, như trường hợp của các trang web trong cấu hình Reverse Proxy nơi các tệp được lưu trữ ở một vị trí khác với ***/var/www***

`site example.com -ssl=on -root-path=/path`

### Bắt buộc WWW hoặc không trong một trang Web

Theo mặc định, Webinoly cấu hình trang web của bạn để chấp nhận cả hai yêu cầu trong tên miền của ban, nghĩa là demo.com và www.demo.com sẽ hợp lệ. Bạn có thể buộc sử dụng và chuyển hướng các yêu cầu đến bất kỳ tùy chọn nào.

`site example.com -force-redirect=<options>`

Option:

* www
* root
* off

### Clone 1 website WordPress

Bạn có thể sao chép bất kỳ trang web WordPress nào.

Tính năng này còn được gọi là các trang dàn dựng của web trong giai đoạn phát triển.

`site example.com -clone-from=dev.example.com`

### Thay thế nội dung

Có thể được sử dụng một mình trong bất kỳ trang web WordPress nào khác để tìm kiếm thay thế bất kỳ từ hoặc chuỗi nào trong nội dung của bạn.

`site example.com -replace-content`

Để sao chép hoặc thay thế nội dung -được kết nối với cơ sở dữ liệu bên ngoài, cần phải nhập tên người dùng và mật khẩu. Để bỏ qua những câu hỏi này, bạn có thể sử dụng tùy chọn `-external-db=[user,pass]`

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

## Command Webinoly 
Câu lệnh `webinoly` cho phép thực hiện một số thay đổi trong cấu hình, cũng như tham khảo một số khía cạnh của máy chủ web.

Cú pháp:

`webinoly <option>`

Option:
* -backup 
* -blockip: Hạn chế quyền truy cập trong Nginx vào 1 IP hoặc khối địa chỉ cụ thể.
  * -blockip=10.10.1.1
  * -block-list: hiển thị danh sách
* -clear-cache : xóa bộ nhớ cache
  * fastcgi:
  * redis
  * memcached
  * opcache
  * all : xóa tất cả

* -conf-value_
* -config-cache
  * -config-cache=[10d,1w,5m]: đầu tiên mã phản hồi, 2 là downtime, 3 là redirects(các chuyển hướng)
* -datadog
* -db-import : Nhập cơ sở dữ liệu MySQL từ dòng lệnh
  * -file=/folder
  * -file=/folder -external-db=[user,pass,host:port]
* -dbpass : Khôi phục tên người dùng và mật khẩu MySQL 
* -default-site=<option>:truy cập vào trình duyệt bằng IP của Server, sẽ thấy mặc định của Nginx.
  * default : Mặc định
  * blackhole : Bất kỳ yêu cầu nào không tương ứng với 1 tên miền hiện có trong máy chủ sẽ trả về mã 444 trong phản hồi hoặc máy chủ sẽ không đáp ứng nhu cầu.
  * domain: Xác định tên miền hoặc trang web hiện tại làm phản hồi mặc định cho bất kỳ yêu cầu nào.
* -external-sources-update
* -header-csp
* -header-hsts
* -header-referrer
* -info
* -login-www-data: người dùng có quyền truy cập hạn cế và chỉ có thể được sử dụng để đăng nhập qua SFTP và chỉ có quyenf truy cập vào các thư mục tệp của trang web.
  * on
  * off
* –mysql-password : Thay đổi mật khẩu mysql
* -query-string-cache: Các URL chứa chuỗi truy vấn không bao giờ được lưu trong bộ nhớ cache theo mặc định. 
* -query-string-never-cache: chỉ định chuỗi truy vấn(nếu có), không bao giờ được lưu vào bộ nhớ đệm.
* -timezone: đặt múi giờ trong PHP và trong hệ điều hành Ubuntu
* -tools-port: thay đổi cổng truy cập PHPMyAdmin
* -tools-site: Định nghĩa một tên miền hiện có cho quyền truy cập vào các công cụ quản lý
* -server-reset
* -skip-cache : loại trừ URL được lưu trữ bởi FastCGI.
  * =/page
* –smtp
* -uninstall
* -update
* -verify
* -version: Nâng cấp phiên bản mới nhất của Webinoly
