#  Giao thức thời gian mạng(NTP)
NTP là giao thưc được sử dụng để đồng bộ hóa thời gian của đồng hồ máy tính trong mạng. Thuật ngữ NTP áp dụng cho cả giao thức và chương trình máy Server - Client chạy trên máy tính.

## NTP hoạt động như thế nào?
* CLient NTP bắt đầu trao đổi yêu cầu thời gian với máy Server NTP 
* Nếu máy Client lệnh giờ với máy Server. Và tự điều chỉnh để khớp với máy Server.
* Sau khi đồng bộ hóa, Client sẽ cập nhật đồng hồ khoảng 10 phút một lần. 

## Các tính năng của NTP
* NTP sử dụng Giờ phối hợp quốc tế(UTC) để đồng bộ hóa thời gian của đồng hồ máy tính với độ chính xác cực cao.
* NTP không tính đến các múi giờ, thay vào đó dựa vào máy chủ để thực hiện các tính toán như vậy.

## Tại sao NTP quan trọng?
* Sự khác biệt giữa 1 giây và phân số có thể gây ra vấn đề. Ví dụ, các quy trình phân phối phụ thuộc vào thời gian phối hợp để đảm bảo của các trình tự phù hợp được tuân theo.

## Giờ UTC và GMT
### GIỜ UTC
* Chuẩn quốc tế về ngày và giờ dựa trên phương pháp nguyên tử.
* Được áp dụng từ năm 1967
### Giờ GMT
* Greenwich Mean Time - GMT được gửi đài thiên văn greenwich mỗi giờ một lần bắt đầu 5-2-1924
* Được tính toán dựa trên sự chuyển động cuaer trái đất xoay quanh trục của nó trong vòng 1 ngày tính từ 12h trưa hôm nay đến 12 giờ trưa hôm sau.
* Quỹ đạo trái đất hình elip gần tròn. Chính vì thế có độ chênh lệnh cao
### Giờ BTS 
* Là giờ mùa hè của nước Anh
* Trước 1 Giờ so với giờ UTC
* Múi giờ này được sử dụng ở châu Âu
### Khác nhau
|UTC|GMT|
|-|-|
|Thời gian do Cơ quan Đo lường và Chất lượng Quốc tế(BIPM) đề xuất làm cơ sở pháp lý cho thời gian. Nó là một phương pháp đo thời giansuwr dụng đồng hồ nguyên tử.|Thơi gian trung bình dựa trên các quan sát thiên văn|
|Là thời gian chuẩn được sử dụng cho một số tiêu chuẩn  Internet và World Wide Web |Được thông qua trong luật pháp của các nước Anh, Bỉ, Canada |

=> Có thể nói rằng UTC là thời gian tiêu chuẩn Thời gian trong khi GM là thời gian theo chuẩn quốc gia.
## Các Lệnh về thời gian
#### Lệnh date
* `date`:Hiển thị 
    * Centos: thứ - tháng - ngày - giờ - múi giờ - năm
    * Ubuntu: thứ - ngày - tháng - năm - giờ  - UTC
* `date -d ` : lùi lại ngày
```
huydv@Ubuntu:~$ date -d 'yesterday'
Wed 20 May 2020 09:13:37 AM UTC
[root@server1 ~]# date -d '4 month ago'
Tue Jan 21 16:15:36 +07 2020
```

* `date -r`: hiện thị thời gian Update mới nhất của file
```
huydv@Ubuntu:~$ date -r /var/log/auth.log
Thu 21 May 2020 05:17:01 PM +07

huydv@Ubuntu:~$ date -r /var/snap/snapd/current/
Fri 15 May 2020 02:28:08 PM +07

[root@server1 ~]# date -r /var/log/cron
Thu May 21 17:01:01 +07 2020

[root@server1 ~]# date -r /var/log/secure
Thu May 21 16:22:36 +07 2020
```
* `date --utc`: Hiển thị thời gian Chuẩn UTC +0
```
[root@server1 ~]# date --utc
Thu May 21 10:25:16 UTC 2020

huydv@Ubuntu:~$ date --utc
Thu 21 May 2020 10:25:12 AM UTC
```
* `date +"%Y-%m-%d %H:%M"`: Lệnh tùy chỉnh đầu ra của date.

#### timedatectl
* `timedatectl`: Hiển thị ngày giờ hiện tại cùng với thông tin chi tiết về cấu hình của đồng hồ và phần cứng
* Thay đổi múi giờ<br>![Imgur](https://i.imgur.com/Xy0IXkB.png)
    * Hiện tại Server  đang thuộc khu vực thời gian là Europe/London.
    * `timedatectl list-timezones`: Xác định múi giờ nào gần với vị trí hiện tại của bạn.
    * `# timedatectl set-timezone Asia/Ho_Chi_Minh`: Thay đổi múi giờ thành `Asia/Ho_Chi_Minh`
    * 

* `timedatectl set-time HH: MM: SS` : Thay đổi thời gian hiện tại.
    * `# timedatectl set-time 23:26:00`: để thay đổi về 11 giờ 26 phút tối
    ![Imgur](https://i.imgur.com/TPkCMxg.png)
* `timedatectl set-time YYYY-MM-DD`: Thay đổi ngày hiện tại.
![Imgur](https://i.imgur.com/qi03Pn3.png)
* `hwclock`
hwclock --set --date "dd mmm yyyy HH:MM"

# hwclock 
* Là tiện ích để truy cập đồng hồ phần cứng, Còn được gọi là đồng hồ thời gian thực(RTC). Đồng hồ phần cứng hoạt động độc lập với hệ điều hành bạn đang sử dụng và hoạt động ngay cả khi tắt máy
* Đồng hồ phần cứng lưu trữ các giá trị của : Năm, Tháng, Ngày Giờ phút và giây. Không thể lưu trữ tiêu chuẩn thời gian 
* Tiện ích lưu các thiết lập của nó trong file `/etc/adjtime`, nó được tạo ra với sự thay đổi đầu tiên khi thực hiện chỉnh sưa thời gian.
* Lệnh sử dụng quyền **root** để sử dụng tiện ích
* `hwclock` : Hiển thị ngày và giờ hiện tại
* `hwclock --set --date "dd mmm yyyy HH:MM"`: Đặt ngày và giờ.
* Đồng bộ hóa ngày và giờ
    * Bạn có thể đồng bộ hóa đồng hồ phần cứng và thời gian hệ thống hiện tại.
        * `hwclock --systohc` 
    * Bạn có thể đặt thời gian hệ thống từ đồng hồ phần cứng.
        * `hwclock --hctosys`
* `# hwclock --systohc --localtime`: Đồng bộ hóa đồng hồ phần cứng với thời gian hệ thống.