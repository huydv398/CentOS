<h3> Cài đặt cấu hình Chrony trên CentOS-7 

## 1. Mô hình chuẩn bị
Chuẩn bị mô hình kết nối

![Imgur](https://i.imgur.com/aXSS0Rm.png)

* Sử dụng 2 Server cho mô hình:
    * CentOS 7
    * Có kết nối Internet
    * Thao tác với tư cách `root`

## Chuẩn bị trước khi cài đặt
Cấu hình Timezone:
```
timedatectl set-timezone Asia/Ho_Chi_Minh
timedatectl
```
Cấu hình allow Firewalld.
```
firewall-cmd --add-service=ntp --permanent 
firewall-cmd --reload 
```
Cấu hình disable SElinux.
```
sudo setenforce 0
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
```
## 3. Cài đặt cấu hình Chrony trên cả 2 server 
Cài đặt Chrony:
`yum install -y chrony`

Sau khi cài đặt chúng ta tiến hành start Chrony và cho phép khởi động cùng hệ thống:
`systemctl enable --now chronyd`

Kiểm tra dịch vụ đang hoạt động .
`systemctl status chronyd`
![Imgur](https://i.imgur.com/83WpnKq.png)

Mặc định trên CenOS7 file cấu hình của Chnory nằm trong file `/etc/chnory.config `, Tiến hành kiểm tra file cấu hình
`cat /etc/chrony.conf | egrep -v '^$|^#'`
Trong đó :
* `server` Xác định các NTP Server bạn muốn sử dụng.
* prefer Đối với nhiều NTP Server chúng ta có thể chỉ định ưu tien kết nối từ NTP Server nào trươc sthay vì để hệ thống tự lựa chọn
* `driftfile` File lưu trữ tốc độ mà đồng hồ hệ thống tăng / giảm thời gian.
* `maketep` Cho phép đồng hồ hệ thống không cập nhật trong 3 bản cập nhật đầu tiên nếu độ lệnh của nó lớn hơn 1 giây.
* `rtcsync` Cho phép đồng bộ hóa kernel của đồng hồ thời gian thực(RTC).
* `logdir Vị trí file log

Các cấu hình bổ sung:
`hwtimestamp` Cho phép đánh dấu thời gian phần cứng trên tất cả các interface hỗ trợ nó.
* `minsources` Tăng số lượng tối thiểu các nguồn có thể chọn cần thiết để điều chỉnh đồng hồ hệ thống.
* `allow` Cho phép Client truy cập NTP từ mạng cục bộ.
* `keyfile` Tệp có chứa mật khẩu để xác thực kết nối giữa Client và Server cho phép Chronyc đăng nhập vào Chronyd và thông báo cho Chronyd về sự hiện diện của liên kết với internet.
`generatecommandkey` Tạo mật khẩu ngẫu nhiên tự động khi bắt đầu Chronyd đầu tiên.
* `log` Log file

## Cấu hình Chrony làm NTP Server 
Chrony cho phép chúng ta cấu hình Server thành một NTP Server. Vieccj này phù hợp cho các mô hình mạng LAN có nhiều máy kết nối. Thay vì phải tốn thời gian đồng bộ từ Internet thì chúng ta có thể cấu hình 1 Server làm NTP server Local và các máy còn lại sẽ đồng bộ thời gian từ NTP Server này về.

Để lựa chọn Pool đồng bộ thời gian chúng ta có thể truy cập vào NTP Poll đề lựa chọn NTP Server. Ở đây chúng ta có thể truy cập vào NTP Pool để lụa chọn NTP Seerver. Ở đây chúng ta giữ nguyên đồng bộ từ `centos.pool.ntp.org`

Tại Server 192.168.20.3 là Server sẽ làm NTP Server. Chúng ta sẽ cấu hình bổ sung cấu hình cho phép các máy Client 192.168.20.2 phái trong có thể đồng bộ hóa thời gian từ Server này.

`# sed -i 's|#allow 192.168.141.0/16|allow 192.168.20.0/24|g' /etc/chrony.conf`

Trong đó 192.168.20.0/24 chính là dải IP local mà chúng ta cho phép các Client kết nối vào NTP Server này để đồng bộ thời gian

Kiểm tra lại file cấu hình:

`cat /etc/chrony.conf | egrep -v '^$|^#'`
![Imgur](https://i.imgur.com/CnDd7vs.png)

Restart lại dịch vụ để cập nhật cấu hình.

`systemctl restart chronyd`

Kiểm tra đồng bộ của `date` và `hwclock` đảm bảo đồng bộ
![Imgur](https://i.imgur.com/IiqDqCn.png)

## Cấu hình Chrony Client
Thực chất sau khi cài đặt và khởi động Chrony thù Server này đã tự động đồng bộ thời gian về từ một trong những NTP SServer thuộc pool `centos.pool.ntp.org`

Bây giờ thay vì đồng bộ thời gian từ Internet chúng ta sẽ đồng bộ từ NTP Server chúng ta cấu hình phía trên 

Tại server 192.168.20.3 Chỉnh sửa cấu hình chrony.
```
[root@server1 ~]# sed -i 's|server 0.centos.pool.ntp.org iburst|server 192.168.20.3 iburst|g' /etc/chrony.conf
[root@server1 ~]# sed -i 's|server 1.centos.pool.ntp.org iburst|server 192.168.20.3 iburst|g' /etc/chrony.conf
[root@server1 ~]# sed -i 's|server 2.centos.pool.ntp.org iburst|server 192.168.20.3 iburst|g' /etc/chrony.conf
[root@server1 ~]# sed -i 's|server 3.centos.pool.ntp.org iburst|server 192.168.20.3 iburst|g' /etc/chrony.conf
[root@server1 ~]#
```
Kiểm tra cấu hình
`cat /etc/chrony.conf | egrep -v '^$|^#'`
![Imgur](https://i.imgur.com/5fq0wBy.png)

Sử dụng `chronyc` kiểm tra đồng bộ.
`# chronyc sources -v`
![Imgur](https://i.imgur.com/RrgWne6.png)

Kiểm tra đồng bộ `timedatectl`
![Imgur](https://i.imgur.com/Uu3AahF.png)

Set đồng bộ thời gian cho đồng hồ của BIOS `hwclock`.

`hwclock --systohc`
Kiểm tra đồng bộ:
![Imgur](https://i.imgur.com/xaseZTy.png)

## Các câu lệnh kiểm tra bổ sung
Kiểm tra verify kết nối

* `chronyc tracking`

![Imgur](https://i.imgur.com/TZLmKD2.png)

* `chronyc sources -v`

![Imgur](https://i.imgur.com/5ZoaNWK.png)

* `chronyc sourcestats`
    * Tại máy Client:

    ![Imgur](https://i.imgur.com/IEn9TSF.png)

    * Tại máy client hiện địa chỉ IP của server khi Pool giờ:
    ![Imgur](https://i.imgur.com/NHdPmmM.png)

## Stop Chrony và kiểm tra 
`systemctl stop chronyd`

`chronyc tracking`

![Imgur](https://i.imgur.com/YTNKX2O.png)

## Tổng kết

Bài viết trên là cơ bản cài đặt cấu hình đồng bộ thời gian  hướng dẫn dưới bài lab của cloud 365
https://news.cloud365.vn/cai-dat-chrony-tren-centos-rhel-7/