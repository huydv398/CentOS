## Chuẩn bị 

Xác định địa chỉ IP Public và địa chỉ IP Private được cấp phát

Kiểm Tra địa chỉ IP đã được sử dụng hay chưa 

### Các bước tạo máy

Chọn Computer 

![Imgur](https://i.imgur.com/5IwJtb6.png)

Chọn Máy KVM
![Imgur](https://i.imgur.com/S98s0xC.png)

### Tạo Ổ cứng
Chọn Storages

![Imgur](https://i.imgur.com/YDJ5j6h.png)

Tạo ở cứng tại vùng muốn chọn.

![Imgur](https://i.imgur.com/k75LMNr.png)

Tạo Ổ cứng

![Imgur](https://i.imgur.com/nY4bSKN.png)

Điền thông tin Ổ cứng 

* Tên Ổ cứng
* Định dạng
* Size

![Imgur](https://i.imgur.com/RUNjvJp.png)

Kiểm tra lại thông tin vừa tạo

![Imgur](https://i.imgur.com/IalTg1p.png)

Tạo máy

![Imgur](https://i.imgur.com/5XfZsRl.png)

![Imgur](https://i.imgur.com/QFkTFBX.png)

Chuyển xang tab Custom 

![Imgur](https://i.imgur.com/Wc06UPC.png)

Điền thông tin máy

![Imgur](https://i.imgur.com/sEX8TAv.png)
* Tên máy
* Số CPU
* RAM

![Imgur](https://i.imgur.com/QR5lq5Z.png)
* Chọn Vùng ổ cứng
* Tên ổ cứng đã tạo ở trên


Thêm Network Interface

![Imgur](https://i.imgur.com/IVPR6AD.png)

Chọn Add network 
* 1: Vlan của địa chỉ IP Public xác định đặt
* 2: Vlan của địa chỉ IP Private xác định đặt

Chọn **Create** để tạo máy


Tạo thành công máy.

![Imgur](https://i.imgur.com/DvQMoS0.png)

Tạo snapshot No ISO

![Imgur](https://i.imgur.com/0O415VB.png)

* Chọn Snapshot
* Điền tên cho Snapshot( ví dụ : None ISO)
* Chọn **Take Snapshot**

Chọn ISO

![Imgur](https://i.imgur.com/2UsWiNq.png)
* Chọn Setting
* Chọn Disk
* Chọn ISO muốn boot cho máy 
* Chọn Mount

Bật Máy 

![Imgur](https://i.imgur.com/I9th8y7.png)

* Chọn Power
* Chọn Power On

![Imgur](https://i.imgur.com/DL30iRx.png)

* Chọn Access
* Chọn Console 

Hiển thị giao diện

![Imgur](https://i.imgur.com/VctaVRr.png) 

## Cài đặt máy chủ ban đầu

Chọn giao Network interfaces làm Interfaces chính trong quá trình cài đặt.

![Imgur](https://i.imgur.com/3yOJth9.png)

Chọn Network Interface : ENS4, interface chính trong quá trình cài đặt

Tạo hostname

Tên của bạn

Tên User login

Mật khẩu

Nhập lại mật khẩu


Cho phép SSH

![Imgur](https://i.imgur.com/NVy0fWv.png)


setup ban đầu thành công

Thiết lập máy chủ:

Tạo mật khẩu cho user **root**

Đặt IP Public và Private

Thay đổi Port SSH

Setup Timedatectl

Tạo snapshot khi đã đặt ip Thành công
