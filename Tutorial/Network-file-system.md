# Network File System - NFS
Network File System - NFS là dịch vụ mạng cho phép bạn truy cập các tệp trên máy chủ được truy cập từ xa giống như cách truy cập vào các tệp máy local. Quyền truy cấp tệp này hoàn toàn dễ hiểu đối với máy CLient và hoạt động trên nhiều kiến trúc máy chủ và máy chủ lưu trữ.
NFS cung cấp một số tính năng hữu ích:
* Dư liệu được truy cập bởi tất cả người dùng có thể được lưu trên một máy server trung tâ, với các máy khách gắn thư mục này vào lúc khởi động.
* Dữ liệu tiêu thụ một lượng lớn dung lượng đĩa có thể được lưu trên một máy chủ.
* Quản trị dữ liệu có thể được lưu trữ trên một máy chủ duy nhất. 
* NFS có cách truy cập bảo mật các hệ thống tệp ở xa, cũng như cách khác nhau để bảo vệ quyền truy cập vào các hệ thống tệp từ xa.
### Các phiên bản và biến thể 
- Khởi đầu NFS(1984)
- NFSv2(3/1989): Cho phép đọc 2GB đầu tiên của tệp tin(Do giới hạn 32Bit)
- NFSv3(1995):Xử lý tệp tin lớn hơn 2GB.
- NFSv4(2000)

## Lợi ích của NFS 
* NFS cho phép truy cập local từ các tệp từ xa.
* Sử dụng kiến trúc Server/Client tiêu chuẩn để chia sẻ tệp.
* Với NFS, Không cần thiết cả hai máy đều cùng chạy trên một hệ điều hành
* NFS tạo ra giải pháo lưu trữ tập trung
* Người dùng có thể lấy được dữ liệu của họ bất cứ đâu
* Có thể được bảo mật với Firewall
### Các tệp quan trọng cho cấu hình NFS
* **/etc/export**: đây là tệp cấu hình chính của **NFS**, tất cả các tệp và thư mục đã Export được xác định trong tệp này ở cuối **máy chủ NFS**.
* **/etc/fstab**: để gắn thư mục NFS trên hệ thống của bạn trên các lần **reboot**, chúng ta cần tạo một mục trong **/etc/fstab**.
* **/etc/config/nfs**: Tệp cấu hình của **NFS** để kiểm soát cổng **RPC** và các dịch vụ đang nghe khác.

## Thiết lập va cấu hình NFS Muonts trên Linux Server 
Để thiết lập các muont NFS, chúng ta sẽ cần ít nhất hai máy Linux/Unix.
Ở đây, tôi sẽ sử dụng 2 máy chủ CentOS,1 máy sử dụng làm server và một máy sử dụng là client.
|Hostname|Interface|IP/sm|gateway|
|-|-|-|-|
|Server: hsv1|ens37|192.168.20.1/24|192.168.20.254|
|Client: cli1|ens37|192.168.20.2/24|192.168.20.254|
### Cài đặt Server của NFS
Chúng tôi cần cài đặt các gói NFS trên máy Server NFS cũng như trên máy Client.<br>
`# yum install nfs-utils nfs-utils-lib` <br>
Bắt đầu các dịch vụ trên cả hai máy.<br>`service nfs start`
### Thiết lập NFS_Server 
Tạo ra 1 thư mục chứa tài nguyên chia sẻ:<br>`# mkdir /shared`<br>
Cấu hình trong thư mục chia sẻ `/etc/exports`. Thêm dòng lệnh<br> `/share 192.168.20.0/24(no_root_squash,no_all_squash,rw,sync)` <br>Trong đó:
* `/share`: là đường dẫn thư mục được chia sẻ
* `192.168.20.0/24`: là dải IP hoặc IP của Client
* `rw`: là quyền truy cập thư mục chia sẻ
* `sync`: đồng bộ hóa thư mục share
* `root_squash`: Vô hiệu hóa được quyền root
* `no_root_squash`: cho phép đặc quyền root
* `no_all_squash`:cho phép người dùng có quyền truy cập

Khởi động dịch vụ `nfs` và `rpcbind`:
```
# systemctl start rpcbind
# systemctl start nfs-server
# systemctl enable rpcbind
# systemctl enable nfs-server
```
Để kiểm tra trạng thái của `nfs` và `rpcbind`:<br>
`systemctl status rpcbind` <br> `systemctl status nfs-server` 

Mở port cho phép truy cập :
```
[root@hsv1 share]# firewall-cmd --permanent --add-service=rpc-bind
success
[root@hsv1 share]# firewall-cmd --permanent --add-service=mountd
success
[root@hsv1 share]# firewall-cmd --permanent --add-port=2049/tcp
success
[root@hsv1 share]#  firewall-cmd --permanent --add-port=2049/udp
success
[root@hsv1 share]# firewall-cmd --reload
success
```
### Thiết lập NFS_Client
Ta tạo ra thư mục `NFS` và **mount** thư mục
```
[root@cli1 ~]# mkdir /demo
[root@cli1 ~]# mount 192.168.20.1:/share /demo
[root@cli1 ~]# ls /demo
file
[root@cli1 ~]# mount | grep /share
192.168.20.1:/share on /demo type nfs4 (rw,relatime,vers=4.1,rsize=524288,wsize=524288,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=192.168.20.2,local_lock=none,addr=192.168.20.1)
```
Tạo 1 tập tin **file.txt**:<br>`[root@cli1 demo]# echo huydv398@gmail.com > file.txt`

Kiểm tra phía Server :
```
[root@hsv1 share]# ls
file.txt
[root@hsv1 share]# cat file.txt
huydv398@gmail.com
[root@hsv1 share]#
```
## Cấu hình Client tự động thư mục được chia sẻ
Các bước trên đã hoàn thành việc share giữa Server và Client, tuy nhiên sau khi hệ thống tắt thư mục shared được mount ở phía Client sẽ bị mất, để tự động mount mỗi khi khởi động ta cần thêm vào file cấu hình `/etc/fstab `như sau:
`echo 192.168.20.1:/share /demo nfs defaults 0 0 >> /etc/fstab`
## Kết luận
* Trên cách bạn có thể thiết lập máy chủ NFS , tạo thư mục dùng chung và xuất nó sang máy khách từ xa;
* Cách cài đặt máy khách NFS và cách liên kết nó với máy chủ NFS của bạn;
* Cám ơn các bạn đã xem

Các link tài liệu tham khảo:
* https://devconnected.com/network-file-system-nfs-administration-on-linux/
* https://www.tecmint.com/how-to-setup-nfs-server-in-linux/