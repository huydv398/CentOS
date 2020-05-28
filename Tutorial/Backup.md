# Sao lưu dữ liệu

#### **Rsync là gì ?**
* **Rsync** - **Remote Sync** :Đồng bộ hóa dữ liệu từ xa.
* Là một công cụ hữu hiệu để sao lưu dữ liệu và đồng bộ hóa dữ liệu trên Linux.
* Nó sẽ chỉ đồng bộ hóa hoặc sao chép các thay đổi từ nguồn đến dích thay vì sao chép toàn bộ dữ liệu, giúp giảm dữ liệu được gửi qua mạng.
#### **Rsync dùng để làm gì?**
* Nó dùng để sao chép hiệu quả và đồng bộ hóa các tập tin đến hoặc từ một hệ thống từ xa.
* Sử dụng giao thức cập nhật từ, chỉ chuyển sự khác biệt giữa các tệp hoặc nội dung thư mục đến đích.

## Câu lệnh.
* Cài đặt `rsync`:
    * `yum install rsync -y`
* Cú pháp câu lệnh: 
    * `rsync [option] [Thư mục nguồn] [Thư mục đích]`
    * Option:
        * `-v`: Hiện thị các tệp tin mới được cập nhật.
        * `-a`: Chế độ lưu trữ, cho phép sao chép đệ quy và nó cũng giữ các liên kết tượng trưng, quyền tệp, quyền sở hữu nhóm và người dùng.
        * `r`: Nói cho rsync biết để copy mọi thứ bao gồm thư mục con và files từ thưu mục gốc. Copy 2 chiều
        * `-b`: tạo bản sao lưu
        * `-e`: Chỉ định shell từ xa để sử dụng
        * `-z`: Nén dữ liệu
        * `-h`: Hiển thị các file và định dạng lên màn hình
        * `--progress`: Hiển thị tiến trình đồng bộ
* Thuật ngữ 
* pull : lấy tài liệu, kéo tài liệu
* push : đẩy, tải lên tài liệu.
### 1. Sao chép, đồng bộ tệp và thư mục tại máy cục bộ
* Chuẩn bị thư mục /home/12/ và có các file dữ liệu
* Mục đích: Đồng bộ hóa dữ liệu vào thư mục /root/huy123/, thay đổi nội dung file và xem sự thay đổi của nó.

* Đồng bộ hóa lần đầu:

![Imgur](https://i.imgur.com/sG8XF2s.png)
* Thay đổi dữ liệu file tại thư mục /home/12 và đồng bộ hóa :
![Imgur](https://i.imgur.com/MbCJjn6.png)

Dòng lệnh hiện thị có 2 file bị có thay đổi về dữ liệu và các file còn không thay đổi.
### 2.Sao chép hoặc đồng bộ hóa các tệp và thư mục từ máy Client(Local) sang hệ máy Server(máy từ xa)
* Chuẩn bị: 
    * 2 máy có cùng dải mạng.
    * Xác định thư mục đồng bộ hóa đến máy từ xa.
* Yêu cầu: Đồng bộ hóa thư mục `/var/log/secure` từ máy client đến máy server tại thư mục `/home/backup`
![Imgur](https://i.imgur.com/4hSEggL.png)

* Kiểm tra lại bên máy Server:
![Imgur](https://i.imgur.com/HJVNvs5.png)
### 3.Sao chép hoặc đồng bộ hóa các tệp và thư mục từ máy Server(máy từ xa) sang máy Client(local).
* Yêu cầu: đồng bộ hóa dữ liệu lấy file `/var/log/yum.log` của máy **Server** sao lưu vào thư mục `/backup/` của máy **Client**
![Imgur](https://i.imgur.com/t1wnISi.png)

### 4.Hiển thị tiens trình đồng bộ óa trong đầu ra lệnh rsync
* Nếu bạn muốn xem tiến trình đồng bộ hoặc sao chép, bạn thêm vào option `--progress`,
![Imgur](https://i.imgur.com/xBWsadh.png)

### 5.Xóa các tệp tại đích nếu không có trong nguồn
* Nếu bạn đã đồng bộ hóa các tệp từ nguồn đến đích và từ nguồn bạn đã xóa  các tệp thì bạn có thể buộc lệnh rsync xóa các tệp trên đích:
![Imgur](https://i.imgur.com/Shvot1t.png)
### 6.Xóa các tệp khỏi nguồn sau khi đồng bộ hóa
```
[root@client huy123]# rsync --remove-source-files -zvh /root/huy123/* root@192.168.20.3:/root/sync
root@192.168.20.3's password:
file1
file10
file11
file12
file13
file14
file15
...
file7
file8
file9
touch

sent 1.29K bytes  received 610 bytes  254.00 bytes/sec
total size is 163  speedup is 0.09
```

* Kiểm tra lại máy Client: tại thư mục nguồn rỗng.

### Bao gồm và loại trừ các tệp tin khi đồng bộ hóa dữ liệu.
Ví dụ trong một thư mục có nhiều file có định dạng khác nhau: **.pdf**, **.png**, **.txt**, **.md**, ...mà bạn lại muốn lấy một loại hoặc lấy hết nhưng không muốn lấy một loại định dạng nào

![Imgur](https://i.imgur.com/Bgi0cMM.png)

* `--include` bao gồm các file có đuôi **.txt** và **.md**, `--exclude` loại bỏ các file có đuôi **.txy** đồng bộ từ local `/root/huy123/` đến `root@192.168.20.3:root/demo1`

## So sánh giữa SCP và Rsync.

* `scp` về cơ bản đọc tệp nguồn và ghi nó đến đích.
* `rsync` cũng sao chép các tập tin cục bộ hoặc qua mạng. Nhưng nó sử dụng một thuật toán chuyển đặc biệt và một vài tối ưu hóa để làm cho hoạt động nhanh hơn nhiều.

* Về bảo mật :
    * `scp` an toàn hơn. Do scp có sử dụng cơ chế ssh
    * `rsync --rsh==ssh` để làm nó an toàn như `scp`

<h3> Trên là phần tìm hiểu về câu lệnh Rsync. Cám ơn các bạn đã xem.