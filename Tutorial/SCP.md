<h3>SCP - Secure Copy

## SCP là gì ?
* **SCP** là tiện ích sử dụng SSH để mã hóa toàn bộ quá trình chuyển tập tin.
* **SCP** lệnh dùng để di chuyển dữ liệu giữa các máy tính chạy hệ điều Linux.
* **SCP** dùng SSH để di chuyển dữ liệu, có chế độ bảo mật giống SSH
## Các mô hình trong SCP.
Với câu lệnh `scp`, bạn có thể sao chép dữ liệu một tập tin hoặc một thư mục:
* Từ hệ thống Server (máy từ xa) đến hệ thống máy Client (máy Local của bạn).
![Imgur](https://i.imgur.com/gChdzv0.png)
* Từ hệ thống Client (máy Local của bạn) đến hệ thống máy Server máy(máy chủ từ xa).
![Imgur](https://i.imgur.com/BhjN8Nh.png)
* Đứng tại máy Client(máy local của bạn) sao chép dữ liệu của hai máy Server, Từ Server 1 đến Server 2 hoặc ngược lại:
![Imgur](https://i.imgur.com/a3CaA0B.png)
## Cú pháp câu lệnh
* `scp [Option] [user]@[IP]:file [user]@[IP]:file2`
    * `Option`: 
        * `-P` : Chỉ định port SSH máy server 
        * `-p` : Bảo toàn sửa đổi tệp tin và thời gian truy cập.
        * `-q` : Nếu bạn muốn chặn đồng hồ đo tiến độ và thông báo lỗi.
        * `-C` : Buộc `scp` nén dữ liệu khi nó được gửi đến máy đích.
        * `-r` : Tùy chọn này sẽ cho biết `scp` để sao chép các thư mục đệ quy.
* Chuẩn bị :
    * 2 máy cài sẵn hệ điều hành
    * 2 máy có kết cùng dải mạng.
    * Xác định thông tin IP, User, Password của server.
### Sao chép tệp từ máy Client sang máy Server.
* Mô hình
![Imgur](https://i.imgur.com/BhjN8Nh.png)
* IPPlanning

![Imgur](https://i.imgur.com/ksYst8v.png)
* Command: `[root@client ~]# scp /root/log/secure-log root@192.168.20.3:/root/`
    * `[root@client ~]`: dùng máy client để thực hiện câu lệnh `scp`.
    * `/root/log/secure-log` đường dẫn tệp muốn sao chép
    * `root@192.168.20.3:/root/` đường dẫn đến là ip: `192.168.20.3`, user là `root`, thư mục chép là `/root/`.
* Tiếp theo, nhập password của user đích.
![Imgur](https://i.imgur.com/yh33WiR.png)

* Lưu thành tên mới tại máy Server, thêm đường dẫn tuyệt đối với tên mới cho file.
    * `[root@client ~]# scp /root/log/secure-log root@192.168.20.3:/root/newfilename`
* Khi muốn copy một thư mục, thêm option `-r`:
    * `scp -r /root/log/ root@192.168.20.3:/root/`

Hoàn thành sao chép một file từ máy Client đến máy Server.
## Sao chép tệp tin, thư mục từ Server sang máy Client
* Mô hình:

![Imgur](https://i.imgur.com/gChdzv0.png)

* IPPlanning

![Imgur](https://i.imgur.com/ksYst8v.png)
* Command: `[root@client ~]# scp root@192.168.20.3:/root/dir/filelog /root/save`
    * `[root@client ~]`: dùng máy client để thực hiện câu lệnh `scp`.
    * `root@192.168.20.3:/root/dir/filelog` đường dẫn tệp muốn sao chép
    * `/root/save/` thư mục chép là `/root/save/`.
* Tiếp theo, nhập password của user đích.
![Imgur](https://i.imgur.com/MoeIkPn.png)

Copy file thành công từ Server về máy Client.
## Đứng tại máy Client(máy local của bạn) sao chép dữ liệu của hai máy Server, Từ Server 1 đến Server 2
* Mô hình:

![Imgur](https://i.imgur.com/0kYIm6w.png)

* IPPlanning:

![Imgur](https://i.imgur.com/XQOvZsk.png)

* Chuẩn bị :
    * 3 máy cài sẵn hệ điều hành
    * 3 máy có kết cùng dải mạng.2 máy làm SCP-Server và 1 máy làm SCP-Client.
    * Xác định thông tin IP, User, Password của server.
* Yêu cầu:
    * Thao tác dòng lệnh tại máy SCP-Client.
    * Tại máy SCP-Server 1, có dữ liệu tại thư mục `/root/bookst/`
    * Tại máy SCP-Server 2, tạo user `huyts` và tạo thư mục có tên `/save/`.
* Command : `[root@client ~]# scp -r root@192.168.20.3:/root/bookst/ huyts@192.168.20.4:/home/huyts/save/`
    * `[root@client ~]` Đứng tại máy Client thao tác câu lệnh.
    * `scp -r`: Sử dụng lệnh để sao chép thư mục.
    * Nguồn sao chép: `root@192.168.20.3:/root/bookst/` : địa chỉ IP: `192.168.20.3` user `root` thư mục `/root/bookst`
    * NGuồn đích dán:`huyts@192.168.20.4:/home/huyts/save/` Địa chỉ IP: `192.168.20.4`, User : `huyts`, thư mục `/home/huyts/save/`

<h3>Trên là phần hướng dẫn sao chép tập tin và thư mục từ máy Client với các máy chủ Server từ xa.Cám ơn các bạn đã đọc.

