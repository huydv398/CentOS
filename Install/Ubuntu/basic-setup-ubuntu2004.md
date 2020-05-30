# Thiết lập máy chủ ban đầu với Ubuntu 20.04
## Giới thiệu
Khi lần đầu tiên tạo máy chủ Ubuntu 20.04 mới, bạn nên thực hiện một số thiết lập cơ bản. Các bước để tăng tính bảo mật và khả năng sử dụng của máy củ của bạn và sẽ cung cấp cho bạn một nền tảng vẫn chắc cho các hành động tiếp theo.

Bước 1 Đăng nhập bằng root
Để đăng nhập vào máy chủ của bạn, bạn sẽ cần biết địa chỉ ip, password, cài đặt khóa key ssh - key private  cho tài khoản root. Nếu bạn chưa đăng nhập vào máy chủ của mình, bạn có thể muốn làm theo hướng dẫn của chúng tôi về cách kết nối droplets với ssh,

## Bước 2 Tạo người dùng mới
Khi đăng nhập bằng root, chúng tôi sẵn sàng thêm tài khoản mới cho người dùng mới. Trong tương lai sẽ đăng nhập tài khoản mới này thay vì đăng nhập tài khoản root.

`# adduser huyts`

Bạn sẽ được hỏi một vài câu hỏi, bắt đầu với mật khẩu. Nhập một mật khẩu mạnh và một số tùy ý thông tin bổ sung có thể Enter để bổ qua
## Bước 3 cấp quyền đặc trị
Siêu người dùng hay trong nhóm sudo có quyền của quản trị viên Người dùng là thành viên nhóm sudo được phép sử dụng lệnh sudo.

`# usermod -aG sudo huyts`

Bây giờ đăng nhập với tư cách người dùng thông thường
## Bước 4 thiết lập tường lửa cơ bản
* Tường lửa UFW để đảm bảo cho phép kết nối tới một số dịch vụ nhất định.
* OpenSSH, dịch vụ cho phép kết nối với máy chủ ngay bây giờ, có một hồ sơ được đăng ký với UFW.

* Bạn có thể thấy điều này bằng cách gõ:

    * `ufw app list`
    * `ufw allow OpenSSH`
    * Kích hoạt tường lửa:`ufw enable`
    * Kiểm tra lại:`ufw status`
* Vì tường lửa hiện tại đang chặn tất cả các kết nối ngoại trừ SSH

## Bước 5: Kích hoạt quyền truy cập bên ngoài cho người dùng thông thường của bạn
* Quá trình định cấu hình quyền truy cập SSH cho người dùng mới của bạn tùy thuộc vào việc tài khoản gốc của máy chỉ của bạn sử dụng mật khẩu hay Key SSH để xác thực
* Nếu tài khoản root sử dụng xác thực mật khẩu, thì xác thực mật khẩu sẽ được bật cho SSH. Bạn có thể SSH vào tài khoản người dùng mới bằng cách mở một phiên cuuoios và sử dụng SSH  với tên người dùng mới :
`$ ssh huyts@IP`
* Sau khi nhập mật khẩu người dung thông thường bạn sẽ đăng nhâp. Hãy nhớ, nếu bạn cần chạy một lệnh với đặc quyền quản trị hãy nhập `sudo` khi nó như thế này

` sudo [câu lệnh mới chạy]`
* Để tăng cường bảo mật máy chủ của bạn, Chúng tôi khuyên bạn nên thiết lập các khóa  SSH thay vì sử dụng xác thực mật khẩu.
Nếu bạn đã đăng nhập vào tài khoản root bằng các khóa SSH