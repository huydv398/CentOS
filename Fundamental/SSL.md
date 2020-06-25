# Giới thiệu
* SSL là chư viết tắt của Secure Sockets Layer (Lớp Socket bảo mật). Một loại bảo mật giúp mã hóa liên lạc giữa web site và trình duyệt. Công nghệ này đang lỗi thời và hoàn toàn thay thế bởi TLS
* TLS - Transport Layer Security, Nó cũng giúp bảo mật thông tin truyền thông như SSL. Nhưng vì SSL không còn được phát triển nữa, nên TLS mới là thuật ngữ đúng tại thời điểm hiện tại.
* Let's Enscrypt là một tổ chức xác thực SSL, là một tổ chức phi lợi nhuận được thành lập với sự bảo trợ của các tổ chức lớn trên thế giới như Cisco, Akamai, Mozilla, Facebook,... với mục đích là cung cấp chứng chỉ SSL miễn phí cho mọi người giúp mọi website đều được mã hóa, tạo nên môi trường Internet an toàn hơn.
* HTTPS là phần mở rộng bảo mật của HTTP. Website được cài đặt chứng chỉ SSL/TLS có thể sử dụng giao thức HTTPS để thiết lập kết nối an toàn với server.

Có 2 cách tạo SSL: 
* Nhờ một tổ chức CA(Certification Authority) cấp cho người sử dụng, là tổ chức có độ tin cậy cao, được quyền cấp và chứng nhận SSL. Sẽ mất phí. 
* Selt-signed SSL: là server tự cấp, tự ký, tự xác thực(không an toàn và tin tưởng bằng nhờ bên thứ 3). Với cách này sẽ không mất phí.

Bài viết sẽ hướng dẫn nhận chứng chỉ SSL miễn phí từ Let's Scrypts và cài đặt SSL trên môi trường Nginx và Cent)S-7.

Tuy nhiên, chứng chỉ SSL trên môi trường Nginx có tác dụng trong vòng 90 ngày. Sau 90 ngày bạn sẽ cần update lại chứng chỉ.

## Chuẩn bị 
1 Server chạy hệ điều hành CentOS-7, đã cài LEMP Stack
