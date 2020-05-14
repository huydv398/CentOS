## Network Advanced
### 1) `ping`
* Lệnh dùng để kiểm tra máy tính có kết nối vơi Internet hay một địa chỉ IP cụ thể nào có kết nối hay không.
* Không giống lệnh ping trên Window, lệnh ping trên Linux sẽ duy trì gửi các gới tin đi bằng cách gõ thêm tùy chọn `-c` hoăc `ctrl`+`c` để kết thúc.
* `# ping -c[number] [IP/domain_destination]`
![image](https://i.imgur.com/ghBO8Z9.png)
### 2) tranceroute
* Dùng để lần dấu đường đi trên mạng tới một đích chỉ định và báo cáo về mỗi `hop` dọc trên đường đi
* Install :`# yum install traceroute -y`
* Command :`# traceroute [options]`
### 3) `mtr`
* `mtr`sẽ liên tục gửi các gói và hiển thời gian `ping` cho mỗi `hop`.
* Câu lệnh cũng giúp phát hiện một số vấn đề mạng qua tỉ lệ **%lose**
* install :`[root@hsv2 ~]# yum install -y mtr`
* Gõ `q` để thoát khỏi tiến trình.
* Command: `# mtr [IP/Domain_destination]`
```
                            My traceroute  [v0.85]
hsv2 (0.0.0.0)                                        Thu May 14 08:14:35 2020
Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                      Packets               Pings
 Host                               Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. 192.168.141.2                    0.0%    25    0.4   0.7   0.3   1.2   0.0
```
### 4) host
* Lệnh `host` sẽ được thực hiện tìm kiếm DNS.
* Nhập vào tên miền khi muốn xem địa chỉ IP đi kèm và ngược lại, nhập vào địa chỉ IP khi muốn xem tên miền đi kèm.
* `# host [IP/Domain]`
```
[root@hsv2 ~]# host 8.8.8.8
8.8.8.8.in-addr.arpa domain name pointer dns.google.

[root@hsv2 ~]# host google.com
google.com has address 216.58.200.78
google.com has IPv6 address 2404:6800:4005:811::200e
google.com mail is handled by 10 aspmx.l.google.com.
google.com mail is handled by 50 alt4.aspmx.l.google.com.
google.com mail is handled by 20 alt1.aspmx.l.google.com.
google.com mail is handled by 30 alt2.aspmx.l.google.com.
google.com mail is handled by 40 alt3.aspmx.l.google.com.

[root@hsv2 ~]# host 103.28.37.50
50.37.28.103.in-addr.arpa domain name pointer srv3750.nhanhoa.com.
