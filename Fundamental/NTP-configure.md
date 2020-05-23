## Cấu hình NTP Server 
https://www.tecmint.com/install-ntp-server-in-centos/
https://www.cyberciti.biz/faq/rhel-fedora-centos-configure-ntp-client-server/
* Cài đặt gói NTP cần thiết trên Server.

    `# yum install -y ntp`
* Đảm bảo các mục sau nằm trong tệp cấu hình `/etc/ntp.conf` 
    * Sử dụng câu lệnh :`cat /etc/ntp.conf` 
    ![Imgur](https://i.imgur.com/EnfFY2n.png)

    * Theo  tệp cấu hình, Các máy NTP Server chỉ dành cho các máy NTP client trên dải mạng `127.0.0.1`

* Bây giờ bạn có thể bắt đầu dịch vụ `ntpd`
    * `# systemctl start ntpd.service `

## Cấu hình máy NTP Client 
* Hiện tại máy NTP Client không chung múi giờ với máy CLient,

![Imgur](https://i.imgur.com/N2S18uO.png)
* Tiến hành cài đặt NTP cho máy Client 
![Imgur](https://i.imgur.com/AE89y7L.png)

* Đối với cấu hình máy Client, hãy thêm cấu hình bên dưới vào tệp `/etc/ntp.conf`