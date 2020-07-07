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