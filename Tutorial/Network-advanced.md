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
* install :`[root@hsv2 ~]#  yum install bind-utils`
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
```
### 5) `whois`
* Đưa ra các bản ghi trên server **whois** của website, vì vậy bạn có thể xem thông tin về người hay tổ chức đã đăng ký và sở hữu trang web đó.
* command: `whos [Domain]`
```
[root@hsv2 ~]# whois nhanhoa.com
   Domain Name: NHANHOA.COM
   Registry Domain ID: 94523481_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.nhanhoa.com
   Registrar URL: http://www.nhanhoa.com
   Updated Date: 2018-09-20T09:53:54Z
   Creation Date: 2003-01-30T16:27:20Z
   Registry Expiry Date: 2028-01-30T16:27:20Z
   Registrar: Nhan Hoa Software Company Ltd.
   Registrar IANA ID: 1710
   Registrar Abuse Contact Email:
   Registrar Abuse Contact Phone:
...
   Name Server: NS2011.NHANHOA.COM
   Name Server: NS2012.NHANHOA.COM
   Name Server: NS2013.NHANHOA.COM
   Name Server: NS2014.NHANHOA.COM
   DNSSEC: unsigned

>>> Last update of whois database: 2020-05-14T02:43:34Z <<<
```

### 6)dhclient
* Giúp làm mới địa chỉ IP trên máy bằng cách giải phóng địa chỉ IP cũ và nhận một địa chỉ ip mới
```
# dhclient -r (release)
# dh client   (renew)
```
### 7) netstat 
* install: `# yum install net-tools`
* Là một lệnh nằm trong số các tệp lệnh để giám sát hệ thống trên Linux.
* Giám sát cả chiều in và chiều out kết nối vào server, Hoặc các tuyến đường route, trạng thái của card mạng.
* Rất hữu ích trong việc giải quyết các vấn đề về sự cố liên quan đến network như là lượng kết nối, traffic, tốc độ, trạng thái của từng port,IP...
* command: ` # netstat [options]`
    * option:
        * `-a`: kiểm tra tổng quan tất cả các port trên server 
        * `-at`: kiểm ra các port chạy **TCP**
        * `-c`: kiểm tra tổng quan tất cả các port trên server ở chế độ runtime.
        * `-au`: kiểm tra các port chạy **UDP**
        * `-l` : kiểm tra các port ở trạng thái `LISTENING` 
        * `-lt` : kiểm tra các port ở trạng thái `LISTENING` đang chạy **TCP**
        * `-plnt` : kiểm tra các port ở trạng thái `LISTENING`đang chạy dich vụ gì.
        * `-lu` : kiểm tra các port ở trạng thái `LISTENING` chạy **UDP**
        * `-rn`: hiển thị bảng định tuyến
        * `-s`: thống kê theo các bộ giao thúc **TCP-UDP-ICMP-IP**
        * `-st` : thống kê theo bộ giao thức **TCP**
        * `-su` : thống kê theo bộ giao thức **UDP**
        * `-i`: hiển thị hoạt động của các network interface
        * `-g`: hiển thị tình trạng IPv4 và IPv6

* Hiển thị các hoạt động của network interface:
```
[root@hsv2 ~]# netstat -i
Kernel Interface table
Iface             MTU    RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
ens33            1500    12635      0      0 0         10832      0      0      0 BMRU
ens37            1500       62      0      0 0            13      0      0      0 BMRU
ens38            1500       62      0      0 0            10      0      0      0 BMRU
lo              65536        8      0      0 0             8      0      0      0 LRU
```
* Hiện thị thông tin bảng định tuyến(Câu lệnh có giá trị như câu lệnh (`route -n`):
```
[root@hsv2 ~]# netstat -rn
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         192.168.141.2   0.0.0.0         UG        0 0          0 ens33
0.0.0.0         192.168.20.254  0.0.0.0         UG        0 0          0 ens37
0.0.0.0         192.168.21.254  0.0.0.0         UG        0 0          0 ens38
192.168.20.0    0.0.0.0         255.255.255.0   U         0 0          0 ens37
192.168.21.0    0.0.0.0         255.255.255.0   U         0 0          0 ens38
192.168.141.0   0.0.0.0         255.255.255.0   U         0 0          0 ens33
```
* Hiện thị các kết nối sử dụng dịch vụ `http`:<br>`[root@hsv2 ~]# netstat -ap |grep http
`
* Hiện thị số lượng gói `SYN_REC` trên Server (nếu có quá nhiều là server đang bị **DDOS** )<br>`# netstat -np | grep SYN_REC | wc -l`
### 8) `tcpdump`
* **TCPDUMP** là phần mềm bắt gói tin trong mẹng làm việc trên hầu hết các phiên bản hệ điều hành Linux. **TCPDUMP** cho phép bắt và lưu lại những gói tin bắt được, từ đó chúng ta có thể sử dụng để phân tích.
* Có thể lưu ra file và đọc bằng công cụ đồ họa **Wireshark**.
* Cài đặt `tcpdump`:<br>`# yum install -y tcpdump`
* command: ` #tcpdump [option] [network_interface]`
    * Option:
        * `-x`: hiện thị nội dung của gói theo dạng `ASCII` và `HEX`
        * `-D`: đầu ra có thể đọc được dòng(để xem khi bạn lưu hoặc gửi đến các lệnh khác) 
        * `-t` : cung cấp đầu ra dấu thời gian có thể đọc được của con người
        * `-q`: Ít dài dòng hơn với đầu ra
        * `-i`: bắt lưu lượng của 1 interface cụ thể
        * `-vv` : Đầu ra cụ thể và chi tiết hơn( Nhiều `v` cho đầu ra nhiều hơn)
        * `-s`: Xác định snaplength(Kích thước) của gói tin theo byte. Sử dụng `-s0` để có được mọi thư. Nếu không set size packet dump thành unlimit, thì khi tcpdump ra nó bị phân mảnh
        * `-c`: chỉ nhận được x số gói và sau đó dừng lại
        * `-S`: In số thứ tự tuyệt đối
        * `-e`: Nhận tiêu đề Internet
        * `-q`: hiển thị thông tin giao thức
        * `-E`: Giải mã lưu lượng IPSEC bằng cách cung câp khóa mã hóa.
* Bắt gói tin có địa chỉ IP cụ thể<br>`# tcpdump -n -i ens33`
    * Sử dụng tùy chon -n để lọc gói tin có chứa địa chỉ IP. Nếu không sử dụng tùy chọn này,`tcpdump`vsex bắt cả các gói tin DNS.

```
[root@hsv2 ~]# tcpdump -n -i ens33 -c4
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens33, link-type EN10MB (Ethernet), capture size 262144 bytes
10:11:54.999790 IP 192.168.141.136 > 216.58.199.14: ICMP echo request, id 9479, seq 222, length 64
10:11:55.000027 IP 192.168.141.136.ssh > 192.168.141.1.55749: Flags [P.], seq 4189737074:4189737138, ack 3500001067, win 452, length 64
10:11:55.000554 IP 192.168.141.1.55749 > 192.168.141.136.ssh: Flags [.], ack 64, win 2048, length 0
10:11:55.000641 IP 192.168.141.136.ssh > 192.168.141.1.55749: Flags [P.], seq 64:224, ack 1, win 452, length 160
4 packets captured
4 packets received by filter
0 packets dropped by kernel
```

* Hiển thị các network interface có sẵn:<br>`# tcpdump -D`
```
[root@hsv2 ~]# tcpdump -D
1.bluetooth0 (Bluetooth adapter number 0)
2.nflog (Linux netfilter log (NFLOG) interface)
3.nfqueue (Linux netfilter queue (NFQUEUE) interface)
4.usbmon1 (USB bus number 1)
5.usbmon2 (USB bus number 2)
6.ens33
7.ens37
8.ens38
9.any (Pseudo-device that captures on all interfaces)
10.lo [Loopback]
```
* Chỉ định số lượng gói tin cần băt(`-c`):<br>`# tcpdump -n -c 3 -i ens33`
* Hiện thị gói tin dưới dạng HEX(`-XX`) và dưới dạng ASCII(`- A`):<br>`# tcpdump -n -XX -i ens33             (HEX)` <br> `# tcpdump -n -A -i ens33             (ASCII)`

* Đọc 1 file .pcap :<br># tcpdump -n -r captured.pcap
* Bắt các gói tin thuộc giao thức cụ thể :<br>`# tcpdump -n -i ens33 [arp|ip|udp|tcp]`
* Bắt gói tin trên port cụ thể :
`# tcpdump -i ens33 port 22`
* Bắt gói tin trên port cụ thể với IP nguồn và IP đích cụ thể :<br>`# tcpdump -i ens33 port 22 [src [IP]|dst [IP]]`