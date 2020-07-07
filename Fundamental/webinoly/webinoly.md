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
