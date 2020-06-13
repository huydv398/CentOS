* Cách nginx và các module hoạt động được xác định trong tệp cấu hình. Theo mặc định, các tập tin cấu hình được đặt tên nginx.conf và được đặt trong thư mục /usr/local/nginx/conf, /etc/nginx hoặc /usr/local/etc/nginx.

## Cấu hình tập tin cấu trúc

nginx bao gồm các module được điều khiển bởi các chỉ thị được chỉ định trong tệp cấu hình. Chỉ thị được chia thành các chỉ thị đơn giản và chỉ thị khối. Một lệnh đơn giản bao gồm tên và thâm số  được phân tách bằng dấu cách và kết thúc bằng ;

Một comment được bắt đầu bằng dấu #
# Phục vụ nội dung tĩnh

Một tác vụ Web Server là phân tán các tệp (như hình ảnh hoặc trang HTML). Bạn sẽ thực hiện một ví dụ trong đó, tùy thuộc vào yêu cầu, các tệp sẽ được phục vụ từ các thư mục local khác nhau: `/data/www` ( có thể chứa HTML) và `/data/images`(chứa hình ảnh). Điều này sẽ yêu cầu tẹp chỉnh sửa cấu hình và thiết lập khối máy chủ bên trong khối http và location.

Đầu tiên, tạo thư mục `/data/www` và đặt nội dung tệp index.html có nội dung văn bản nào đó vào tạo ra thư mục `/data/image` và đặt một số hình ảnh trong đó.

Mở tập tin cấu hình. Tệp cấu hình mặc định đã bao gồm một số ví dụ về `server`, chủ yếu được nhận xét. Bây giờ hãy nhận xét tất cả các khối như vậy và bắt đầu bằng một khối `server` mới:
```
http {
    server {
    }
}
```


## Nhật ký hệ thống
Các  lệnh error_log và access_log hỗ trợ ghi nhật ký vào syslog

`server=address` 
* Xác định địa chỉ của một nhật ký hệ thống của máy chủ. Địa chỉ có thể được địa chỉ IP hoặc domain