# TCP Wrappers là gì?
* Là một hệ danh sách kiểm soát truy cập mạng dựa trên IP được sử dụng để lọc truy cập mạng tới các máy chủ Linux.
* Nó cho phép host hoặc mặt nạ mạng con, hostname và phản đối truy vấn giao thức Ident được sử dụng làm token lọc cho mục đích kiểm soát truy cập.
* TCP Wrappers thêm một lớp bảo vệ bổ sung bằng cách xác định máy chủ nào đang hoặc không được phép kết nối với các dịch vụ mạng đang được Wrapper

## TCP Wrappers
* Các gói TCP Wrappers được cài đặt theo mặc định và cung cấp kiểm soát truy cập dựa trên máy chủ cho các dịch vụ mạng.
* Thành phần thư viện: `/lib/libwrap.so` hoặc `/lib64/libwrap.so` 
## Hoạt động TCP Wrappers
* Khi một kết nối cố gắng thực hiện cho dịch vụ wrapper,trước tiên dịch vụ sẽ xem xét các tệp của máy chủ(`/etc/hosts.allow` và `/etc/host.deny`) để xem máy khách có được phép kết nối hay không. 
* Nếu máy Client được phép kết nối, TCP Wrapper sẽ giải phóng quyền kiểm soát kết nối đến dịch vụ được yêu cầu tham gia thêm vào giao tiếp giữa máy server và client.

## Cài đặt TCP Wrappers
TCP Wrappers có sẵn trong kho chính thức của hầu hết các hệ điều hành linux.
* Cài đặt trên CentOS:

` yum -y install tcp_wrappers `

* Cài đặt trên Ubuntu:

` sudo apy-get install tcp_wrappers`

## Hạn chế quyền truy cập vào máy chủ Linux bằng cách sử dụng TCP Wrappers 
### Cấu hình 
TCP thực hiện kiểm soát với truy cập với sự trợ giúp của hai tẹp cấu hình:
* `/etc/hosts.allow`: Tệp này chứa tên các máy chủ được phép sử dụng các dịch vụ mạng.
* `/etc/hosts.deny`: Tệp này chứa tên các máy chủ không thể sử dụng dịch vụ mạng.
* Nếu cùng một máy client, người dùng hoặc IP được liệt kê trong cả hai tệp, Hosts.allow có mức độ ưu tiên hơn hosts.deny.
### Cú pháp của tệp hosts.allow và hosts.deny
Cú pháp của các tệp này như sau:

` list_of_service : list_of_client [:lệnh_ shell]`

Trong đó:
* `service _of_list` là danh sách các tên tiến trình của trình nền cần xem xét.
* `list_of_client` là danh sách tên máy chủ, địa chỉ IP, mẫu đặc bietj hoặc ký tự đại diện sẽ được so sánh với từng máy khách được kết nối.
### Cách tiếp cận được đề xuất để bảo mật máy chủ của bạn
* Mô hình:

![Imgur](https://i.imgur.com/Wn4FAz1.png)

* IPPlanning:

![Imgur](https://i.imgur.com/NjPyZ9Y.png)

* Vấn đề: Chỉ cho phép máy có địa chỉ IP 192.168.20.3 truy cập.
Để bảo mật máy chủ linux là chặn tắt cả các kết nối đến và chỉ cho phép một vài máy chủ hoặc mạng cụ thể. 

Để làm vậy, chỉnh sửa tệp `/etc/hosts.deny`:

`sudo vi /etc/hosts.deny`

* Thêm dòng sau. Dòng này từ chới kết nối với tất cả các dịch vụ và tất cả ccs mạng.
    * `ALL: ALL`    

Sau đó, chỉnh sửa tệp `/etc/hosts.allow`:

`sudo vi /etc/host.allowsudo`

* Thêm địa chỉ mạng muốn kết nối:

`sshd : 192.168.20.4`

Theo quy tắc trên, tất cả các kết nối đến đều bị từ chối cho tất cả các máy chủ ngoại trừ 192.168.20.4

* Bạn có thể xác minh điều này từ nhật ký của máy chủ linux của bạn như hiển thị bên dưới.

`cat /var/log/secure`

#### Kiểm tra

* Máy có địa chỉ IP 192.168.20.4 SSH đến máy server .

![Imgur](https://i.imgur.com/j9uO5i6.png)

![Imgur](https://i.imgur.com/Cn3GFYN.png)

* Máy có địa chỉ 192.168.20.140 SSH đến server.

![Imgur](https://i.imgur.com/p9TaYU9.png)

Không thể SSH tới máy Server

* Cho phép tất cả các máy chủ ngoại trừ một máy chủ cụ thể

Bạn có thể cho phép các kết nối đến từ tất cả các máy chủ, nhưng không cho phép từ chủ cụ thể. Ví dụ, để cho phép các kết nối đến 

Ví dụ :
* Mô hình:

* IPPlanning

Cho phép tất cả các kết nối trong dải 192.168.20. ,nhưng không cho máy có địa chỉ 192.168.20.4.