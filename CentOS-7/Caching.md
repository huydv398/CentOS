Lab Nginx Reverse Proxy & Caching

## Giới thiệu

Về Cache: Nhiệm vụ chung của cache giúp tăng tốc độ truy cập dữ liệu và giảm tắc nghẽn về băng thông khi có quá nhiều người dùng cung truy cập đồng thời vào dữ liệu web.

Với Nginx đóng vai trò là cache: Kỹ thuật Cache trong Nginx được sử dụng để việc cải tiến tốc độ truy cập web hoặc ứng dụng trong các giải pháp CDN(Content Delivery Network)

Mô hình

Trong mô hình thực hiện các cấu hình sau:

1. Cài đặt máy web sẻver sử dụng Apache, sau đó up nội dung các file tạo site đơn giản
2. Cài đặt Nginx làm chức năng Reverse Proxy và Caching.
3. Người dùng mở trình duyệt hoặc dùng lệnh curl với tùy chọn -l để kiểm tra xem proxy có hoạt động hay không.
4. Người dùng truy cập nhiều lần vào web hoặc dùng curl để kiểm tra xem đã cache được chưa?

## Triển khai

### Cài đặt Apache trên máy web-server 

Thiết lập IP theo mô hình.

Thực hiện trên máy 2.2.2.3/24

Thực hiện cài đặt các gói bổ trợ với quyền root

```
yum update -y
yum install -y epel-release 
yum install -y wget byobu 
```


Kiểm tra nội dung web và xem reverse proxy đã hoạt động hay chưa bằng cách truy cập vào địa chỉ huynet.com trên thanh URL. Trong ảnh này hiển thị được nội dung của Web-server.
![Imgur](https://i.imgur.com/AIWCv20.png)

![Imgur](https://i.imgur.com/SdgQIvu.png)

Hoặc có thể sử dụng lệnh `curl -I` để kiểm tra
![Imgur](https://i.imgur.com/7M6w5tG.png)

### Cấu hình caching cho Nginx

