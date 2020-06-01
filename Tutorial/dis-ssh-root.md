# Tạo user sudo và cấu hình disable ssh đối với tài khoản user root.

Lệnh sudo là gì ?
* `sudo`- (Superuser do) là một tiện ích dành cho hệ thống Linux, hiệu quả để cung cấp cho người dùng cụ thể, quyền để sử dụng các lệnh hệ thống cụ thể ở cấp độ root. `sudo` cũng ghi lại tất cả các lệnh và tham số.Sử dụng sudo, quản trị viên có thể:
    * cung cấp cho một số người dùng (hoặc nhóm người dùng) có khả năng- chạy một hoặc tất cả các lệnh ở cấp độ root của hoạt động hệ thống.
    * Kiểm soát lệnh nào người dùng có thể sử dụng trên mỗi máy chủ.
    * Xem rõ từ nhật ký(`log`) mà mà người dùng đã sử dụng lệnh nào.
    * Sử tệp timestamp, kiểm soát thời gian mà người dùng phải nhập lệnh sau khi họ nhập mật khẩu và được cấp quyền.

## Tạo và cấp quyền sudo cho người dùng mới.
Để tạo và cấp quyền cho sudo cho người dùng mới, login server bằng user root
### 1. Tạo user và cấp password cho user đó.
* Tạo user mới vào hệ thống.
    * `# adduser duonghuy`
* Cập nhật mật khẩu cho người dùng mới.
    * `# passwd duonghuy`

    ![Imgur](https://i.imgur.com/D0EItEU.png)

### 2. Cấp quyền sudo cho user vừa tạo
* Sử dụng câu lệnh `usermod` để thêm người dùng vào nhóm `wheel`.
    * `usermod -aG wheel duonghuy`
        * `usermod` : câu lệnh 
        * `-aG`: Thêm người dùng vào nhóm
        * `wheel`: Nhóm có sẵn trong Server, các thành viên được thêm vào nhóm sẽ có đặc quyền sudo.
        * `duonghuy`: Tên user được thêm vào nhóm.
### 3. Kiểm tra lại người dùng với quyền sudo.
* Sử dụng lệnh sau để kiểm tra xem người dùng đã được add vào nhóm wheel hay chưa.
    * `# cat /etc/group | grep duonghuy`

    ```
    wheel:x:10:duonghuy
    duonghuy:x:1002:
    ```
## Cấu hình disable ssh đối với tài khoàn root
### 1. Vô hiệu hóa đăng nhập từ xa đối với user root
* Muốn vô hiệu hóa user root đăng nhập ssh, ta vào file `vi /etc/ssh/sshd_config `

* Tìm đến dòng có chứa nội dung `#PermitRootLogin yes` và chỉnh sửa thành `PermitRootLogin no`, lưu và thoát file.
* Khởi động lại sshd để áp dụng cấu hình
    * `systemctl restart sshd`

### 2. Kiểm tra lại đăng nhập đối với user root
* Đăng nhập bằng user root.
![Imgur](https://i.imgur.com/mj2tvYJ.png)
* Hệ thống sẽ không thể SSH từ xa đăng nhập bằng tài khoản root. Nhưng tài khoản vẫn có thể đăng nhập bằng tài khoản người dùng.