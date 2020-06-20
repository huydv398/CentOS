# WORLD WIDE WEB: HTTP

1990 Internet được sử dụng trong các cơ quan nghiên cứu, trường đại học với các dịch vụ đơn giản như truy cập từ xa, truyền file, nhận và gửi thư điện tử. WWW xuất hiện, nhanh chóng được mọi người đón nhận. Nó thay đổi cách thức tương tác giữa con người và môi trường làm việc.

## Tổng quan về HTTP

    Hyper Text Protocol (HTTP)- giao thức tầng ứng dụng của web - là trái tim của web. HTTP được triển khai trên cả phía client và server.Giao tiếp với nhau qua việc trao đổi thông điệp HTTP.

**Web** (trang web, webpage, website- một tập tin) chứa các đối tượng đơn giản như một file HTML, file hình ảnh,... Đối tượng được xác định qua url. 

Đây là là một địa chỉ URL :

`www.example-web.com.vn/image/banner/image.jpg` 

www.example-web.com.vn là tên máy chủ và image/banner/image.jpg là đường dẫn đối tượng

**Trình duyệt**(Browser) - chương trình giao tiếp người dùng của ứng dụng web cho phép hiển thị trang web. Browser là phía client của giao thức HTTP. Hiện nay có rất nhiều trình duyệt web phổ biến là Chorme , Coc Coc, Fire Fox, ... Web server lưu trữ các đối tượng web và được xác định qua địa chỉ url. Một số phần mềm phổ biến Apache, Nginx,...

![Imgur](https://i.imgur.com/3HnYojM.png)

HTTP xác định cách thức trình duyệt yêu cầu trang web từ webserver 

#### Kết nối liên tục và kết nối không liên tục(persistent/ nonpersistent)

Chế độ mặc định của HTTP 1.0 : Sử dụng kết nối không liên tục.

Chế độ mặc định của HTTP 1.1 : Sử dụng kết nối liên tục

**Kết nối không liên tục**
* HTTP Clent khởi tạo kết nối TCP tới Server với domain.
* HTTP Clent gửi thông điệp yêu cầu tới TCP
* HTTP Server nhận được thông điệp yêu cầu, lấy đối tượng trong ổ cứng hoặc ram, đặt đối tượng vào một thông điệp trả lời và gửi đi.
* HTTP Server kết thúc kết nối.
* HTTP Clent nhận được thông điệp trả lời, đóng kết nối.

4 bước đầu được lặp lại cho mỗi đối tượng ảnh được tham chiếu trong file HTML.

**Kết nối liên tục**

 Có một vài nhược điểm trong kết nối không liên tục: khi kết nối mới được tạo client và server phải tạo vùng đệm TCP cũng như lưu trữ các biến TCP - là gánh nặng cho server khi có nhiều client cùng lúc.

Với kết nối liên tục, Server không đóng liên kết TCP sau khi gửi thông điệp trả lời. Các thông điệp yêu cầu và trả lời sau đó được gửi qua cùng một kết nối. HTTP server đóng liên kết khi liên kết không được sử dụng trong một khoảng thời gian nào đó.

### Khuân dạng thông điệp HTTP
####  Thông điệp yêu cầu.
```
GET / HTTP/1.1" 
"Mozilla/5.0 (Windows NT 10.0; Win64; x64) 
AppleWebKit/537.36 (KHTML, like Gecko) 
Chrome/83.0.4103.106 Safari/537.36
```
Dòng đầu tiên dòng yêu cầu các dòng sau là dòng tiêu đề header

Dòng yêu cầu có 3 trường Method, URL và phiên bản HTTP.  Trường Method nhận một trong 3 giá trị: **GET**, **POST** và **HEAD**. Phần lớn sử dụng phương thức **GET**. Với phương thức **POST** người dùng yêu cầu trang Web nhưng nội dung cụ thể phụ thuộc vào nội dung điền trong form. Phương thức **HEAD** thường dùng sử dụng để phương thức **HEAD** để gỡ lỗi.