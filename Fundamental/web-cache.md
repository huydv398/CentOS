# Web cache
![Imgur](https://i.imgur.com/D7D4qy5.png)

* Web cache (proxy server) là thực thể đáp ứng yêu cầu từ client. Máy tính làm nhiệm vụ Web cache có ổ đĩa riêng lưu trữ bản sao các đối tượng đã từng được yêu cầu

Với mô hình ở trên người sử dụng cấu hình trình duyệt sao cho tất cả các yêu cầu đều được gửi đến web cache trước. Khi đó tất cả các yêu cầu về một  đối tượng nào đó sẽ được chuyển đến web cache trước. Giả sử trình duyệt yêu cầu đối tượng là một file ảng có địa chỉ: **http://www.example.com/image/picture1.png**

* Trình duyệt khởi tạo một kết nối tới TCP với Web cache và gửi yêu cầu tới web cache.
* web cache sẽ kiểm tra và tìm đối tượng, Nếu tìm được thì webcache sẽ gửi đối tượng qua client qua kết nối TCP đã được thiết lập.
* Nếu Webcache không có đối tượng đó thì nó sẽ khởi tạo một kết nối tới server thật chứa đối tượng, ở đây **là www.example.com**. Sau đó webcache gửi thông điệp yêu cầu cho server thông qua TCP vừa khởi tạo. Sau khi nhận được yêu cầu từ webcache, server sẽ gửi lại đối tượng cho web cache.

* Khi nhận được đối tượng, Webcache sẽ lưu lại bản sao của đối tượng và gửi đối tượng trong thông điệp HTTP trả lời cho máy Client(thông qua kết nối TCP được thiết lập trước đó).

Webcache được sử dụng rộng rãi với 3 lý do sau:
* Web giảm thời gian client phải đợi. Trong trường hợp cache có đối tượng này sẽ ngay lập tức được chuyền cho Client.

* Webcache làm giảm tải mạng. Bằng cách giảm tải đường truyền ra mạng Internet, Cơ quan không cần phải nâng cấp đường truyền  và giảm chi phí. Webcache làm giảm chất lượng thông tin trao đổi trên Interne, do đó tăng hiệu xuất hoạt động của tất cả các ứng dụng.
* Mạng Internet với nhiều webcache giúp cho việc nhanh chóng phát tán thông tin - thậm chí ngay cho nhà cung cấp thông tin có tốc độ server chậm hay tốc độ kết nối chậm. Nếu nhà cung cấp có nội dung cần phổ biến thì nội dung này ngay lập tức được chuyển đến các web cache và yêu cầu người dùng từ mọi nơi được đáp ứng nhanh chóng.


