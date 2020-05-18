## IP Ubuntu
* Trước khi đi vào cài đặt IP cho Ubuntu, giới thiệu về Netplan: là một công cụ cấu hình mạng được giới thiệu trong Ubuntu. Nó có thể được sự dụng để mo tả YAML(là một chuẩn dữ liệu thông tin để tất cả các ngôn ngữ có thể đọc được) đơn giản về giao diện mạng với những thông tin cơ bản và tạo ra cấu hình cần thiết cho một công cụ kết xuất được chọn.
* Có 2 cách để đặt địa chỉ IP cho Interface trên Ubuntu:

## Cấu hình bằng NETPLAN
Trong mô hình lab, tôi đã cài đặt xong máy Ubuntu 20.04 với mặc định một card mạng ens33 ở chế độ NAT. Bây giờ sẽ tiến hành gắn thêm một cấu hình card mạng và cấp IP cho nó. Kiểm tra thiết bị đang có trong mạng, bằng câu lệnh:<br> `ip a` <br>
![Imgur](https://i.imgur.com/yJTIIk8.png)
Card Ens38 đã tồn tại tuy nhiên nó chưa được cấp địa chỉ IP. Tiếp theo tôi sẽ cấu hình cho nó vằng cách vào thư mục `00-installer-config.yaml`. (Lưu ý khi viết vào tệp yaml bạn phải tuân thủ theo đúng cú pháp căn dòng của nó). Ở đây tôi muốn cấp IP động nên sẽ ghi vào nội dung file như sau:<br> `huydv@hsv:~$sudo vi /etc/netplan/00-installer-config.yaml`
## Đặt IP động
* Thêm tên card mạng và loại hình mạng
```
    ens38:
            dhcp4: true
```
![Imgur](https://i.imgur.com/ZQO9Tsv.png)
* Lưu lại cấu hình và restart lại Network bằng câu lệnh `netplan apply`
### Đặt IP tĩnh 
Trong trường hợp muốn đặt IP tĩnh, thay đổi lại cấu hình interface như sau:
* Xác định thông số muốn đặt cho Interter trước khi đặt, ở đây tôi sẽ đặt IP cho Interface như sau:
    * IP: 192.168.10.10/24
    * Gateway: 192.168.10.254
    * DNS: 8.8.8.8
>***Chú ý** đặt đúng cấu trúc của file yaml
```
ens38:
            dhcp4: no
            addresses: [192.168.10.10/24]
            gateway4: 192.168.10.254
            nameservers:
                    addresses: [8.8.8.8]
```
![Imgur](https://i.imgur.com/o9hzi4e.png)
* Lưu thay đổi file và chạy lệnh `netplan apply` để thiết lập lại cấu hình.
* Dùng lệnh `ip a` kiểm tra :
![Imgur](https://i.imgur.com/FKooc3H.png)
Đã thành công việc đặt địa chỉ IP cho Ubuntu 20.04 thông qua cấu hình **netplan**
## Sử dụng `ifupdown` đêt đặt IP cho Ubuntu
Các thao tác phải được thực hiện với quyền root
### Disable Netplan
* Tắt Netplan:
Sử dụng dòng lệnh: <br>`echo 'GRUB_CMDLINE_LINUX = "netcfg/do_not_use_netplan = true"' >>  /etc/default/grub`
* Cập nhật lại grub: <br> `update-grub`
### Cài đặt `ifupdown` thay thế `netplan`
* Cài đặt `ifupdown` Bằng câu lệnh:<br> `apt-get update` <br> `apt-get install -y ifupdown`
### Xóa `netplan` khỏi hệ thống:
* Sử dụng lệnh:<br> `apt-get --purge remove netplan.io` <br> Nếu bạn chưa cài đặt `ifupdown`, khi xóa bỏ netplan hệ thống sẽ tự cài `ifupdown` thay thế cho bạn.<br> Sau đó ta xóa toàn bộ cấu hình của netplan:<br> 
`rm -rf /usr/share/netplan` <br> `rm -rf /etc/netplan`
### Cấu hình interface
**File cấu hình interface**: `/etc/network/interfaces`
* Chỉnh sửa File cấu hình:<br> `vi /etc/network/interfaces`
* Thêm các dòng sau vào file cấu hình:
    * Chú ý cần xác định đúng tên interface và các thông số về IP của bạn.
    ```
    auto lo
    iface lo inet loopback

    auto ens38
    iface ens38 inet static
    address 192.168.10.11
    netmask 255.255.255.0
    gateway 192.168.10.254
    broadcst 192.168.10.255
    dns-nameservers 8.8.8.8 8.8.4.4
    ```
![Imgur](https://i.imgur.com/7IhUmMi.png)
* `reboot` hệ thống.
### Kiểm tra kết nối mạng
Sau khi reboot. Ta kiểm tra lại IP đã đặt
![Imgur](https://i.imgur.com/AH8mnyJ.png)