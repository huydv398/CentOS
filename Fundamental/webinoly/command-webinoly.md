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
