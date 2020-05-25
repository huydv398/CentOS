## Thiết lập cơ bản
### Đặt hostname cho CentOS-7
* Câu lệnh: `hostnamectl set-hostname [name-server]`
## Đặt ip tĩnh cho CentOS7
Yêu cầu 2 Máy được cài hệ điều hành linux.
### 1. Mô hình dựng LAB
![Imgur](https://i.imgur.com/8t1u22k.png)
* Có dải IP cho các Interface như sau:<br>
<img src=https://imgur.com/5C3nHwQ.png><br>
![Imgur](https://i.imgur.com/wfRWm0y.png)

### Các bước cài đặt
* Server `hsv1`:
    * Thay đổi `connection.id` cho trùng tên với `DEVICE`
    Với máy Server `hrv1`:
    <img src=https://imgur.com/6GLvgj7.png>

    * Đặt IP cho các Interface:<br><img src=https://imgur.com/WxwPtAY.png>
    * `# nmcli conn m ipv4.method manual`: thêm câu lệnh này để khi khởi động sẽ giữ lại địa chỉ IP. Nếu không server sẽ dhcp địa chỉ như ban đầu.
    * Để Interface tự động bật khi khởi động lại ta có câu lệnh:<br><img src=https://imgur.com/WWqSnDO.png>
    * `reboot` để khởi động lại hệ thống
* Server `hsv2`:
      * Thay đổi `connection.id` cho trùng tên với `DEVICE`
    Với máy Server `hrv1`:
    <img src="https://imgur.com/twG9TfC.png">
    * Đặt IP cho Interface:<br>![](https://imgur.com/7YsJduI)
    * `# nmcli conn m ipv4.method manual`: thêm câu lệnh này để khi khởi động sẽ giữ lại địa chỉ IP. Nếu không server sẽ dhcp địa chỉ như ban đầu.
    * reset lại network :`# service network restart`

## Kết luận
Trên là phần đặt ip tĩnh cho server CentOS-7. Cám ơn các bạn đã xem.