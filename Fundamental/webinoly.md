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

Việc cài đặt sẽ mất khoảng 5-7 phut. Sau khi cài đặt xong, kết quả

## Command **site**
* Dùng để tạo bất kỳ một trang web mới của bạn, thêm chứng chỉ SSL, kích hoạt FastCgi Cache cho cài đặt WordPress của bạn và nhiều tính năng khác sẽ cho phép bạn có toàn quyền kiểm soát các trang web của mình một cách dễ dàng và đáng tin cậy, luôn luôn ở một lệnh 

`sudo site <domain> <option> <option2>`

Option:
* -cache
* -clone-from
* -delete : xóa bỏ
* -delete-all: Xóa hết
* -force-redirect=(Option)
    * www:
    * root
    * off
* -forward
* –html: Tạo trang web HTML
* -list: Hiển thị các trang web đã tạo
* -multisite-convert
* –mysql: Tạo một trang web php kết hợp với cơ sở dữ liệu
* -on
* -off
* -parked
* –php
* -proxy
* -redirection
* –ssl
* -wp
* –yoast-sitemap
### Tạo một trang web mới
Tạo một trang web mới thuộc bất kỳ loại nào: HTML, PHP, WordPress, Reverse Proxy, v.v.

Tạo một trang HTML cơ bản:

`site demo.com -html`

Tạo một trang web có hỗ trợ PHP

`site demo.com -php`

Tạo một trang web PHP kết hợp với cơ sở dữ liệu:

`site demo.com -mysql`

Tùy chọn chỉnh sửa web trước khi tạo:

`site web.duonghuy.xyz  -mysql=custom`

Câu lệnh trên dùng để tạo một trang web và người tạo phải truyền vào dữ liệu của MySQL

### Tạo 1 trang WordPress
`site domain -wp`

![Imgur](https://i.imgur.com/g3tVIzb.png)

Sử dụng lệnh sau để tắt login admin khi truy cập

`httpauth demo.com -wp-admin=off`

![Imgur](https://i.imgur.com/xvLGf4i.png)

![Imgur](https://i.imgur.com/QMfLd4B.png)

![Imgur](https://i.imgur.com/9DpR3Of.png)

![Imgur](https://i.imgur.com/oAdCcrk.png)
 

### WordPress Multisite
Chuyển đổi trang WordPress sang [WordPress Multisite](Note/multisite.md)

`site example.com -multisite-convert`

Nó 

### Bộ nhớ cache FastCGI

Cùng với Nginx tối ưu hóa để tăng tốc trang web WordPress của mình. 

Để bật FastCGI:

`sudo site example.com -cache=on`

Để tắt FastCGI:

`sudo site example.com -cache=off`

Cũng có thể kích hoạt nó từ việc tạo trang mới

`sudo site example.com -wp -cache=on`

### Cấu hình thư mục con
??
### Tên miền chưa được sử dụng hoặc bí danh
Một tên miền chưa được sử dụng là một tên miền bổ sung hoặc thay thế trỏ đến trang web chính. Đó là một cách đơn giản để truy cập trang web của bạn từ các tên miền khác nhau.

`sudo site example.com -parked`

Khi thực hiện lệnh, bạn sẽ được yêu cầu nhập tên miền chính nơi tên miền chưa được sử dụng mới sẽ trỏ đến. Theo cùng một cách bạn có thể sử dụng lệnh sau để tạo điều kiện cho việc sử dụng nó:

`sudo site example.com -parked=mainsite.com`

Đảm bảo trang web chính được lưu trữ trên cùng một máy chủ.

### Chuyển tiếp tên miền 

Tất cả các yêu cầu cho tên miền này sẽ được chuyển hướng hoặc chuyển tiếp đến tên miền khác.

`sudo site example.com -forward=example.org`

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



