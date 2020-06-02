# History
* Lệnh `history` được dùng để kiểm tra lịch sử lệnh hoặc để có dược thông tin về lệnh được thực thi bởi người dùng.
* Cho phép nhanh chóng xem những gì đã được thực hiện trước đây trên hệ thống, cho phép người dùng chịu trách nhiệm cho hành động của họ
## Sử dụng
### `history`
* Lệnh dùng để hiển thị tất cả các lịch sử lệnh.
```
[root@srv01 ~]# history
    1  hostnamectl set-hostname srv01
   
   14  targetcli
   15  timedatectl status
   16  ip a
   17  vi /var/log/secure
   18  add user
   19  useradd huy
   ...
   30  nmcli connection modify ens37 ipv4.dns 8.8.8.8
   31  nmcli connection modify ens37 ipv4.method manual
   32  nmcli connection modify ens37 connection.autoconnect yes
   ...
   39  adduser duonghuy
   40  passwd duonghuy
   41  ip a
   42  cat /etc/os-release
   43  passwd duonghuy
[root@srv01 ~]#
```
### Tìm kiếm command đã sử dụng trong quá khứ.
* Muốn tìm 1 lệnh nhất định thì sử dụng thêm lệnh `grep` để lọc ra kết quả mong muốn.
![Imgur](https://i.imgur.com/ciaLtLH.png)

### Thực hiện các lệnh trong quá khứ.
![Imgur](https://i.imgur.com/AmqMHZ4.png)

Muốn thực hiện lệnh 32, cú pháp như sau:

`# !32`

![Imgur](https://i.imgur.com/ONcfTFQ.png) 
Bash sẽ chạy lệnh số 32 trong lịch sử câu lệnh

`!!` : Thực hiện lệnh mà bạn vừa sử dụng trước đó.
### $HISTSIZE, $HISTFILE, $HISTFILESIZE
* ![Imgur](https://i.imgur.com/l0xCRtI.png)
    * Biến $HISTSIZE xác định số lượng lệnh sẽ được ghi nhớ trong môi trường hiện tại của bạn

* ![Imgur](https://i.imgur.com/6XVz8zX.png)
    * Biến $HISTFILE trỏ đến tệp chứa lịch sử của bạn.

* ![Imgur](https://i.imgur.com/9Zpu2gW.png)
    * 1000 - Là số lịch sử hệ thống có thể lưu lại được.


### Thay đổi format của output history 

`$ HISTTIMEFORMAT="%d/%m/%y %T "`

![Imgur](https://i.imgur.com/eTnU9v3.png)

`$ HISTTIMEFORMAT="%T %d/%m/%y "`  

![Imgur](https://i.imgur.com/K4d2iun.png)
* option:
    * %d - Ngày 
    * %m - Tháng
    * %y - Năm
    * %T - Thời gian
    * `/` : có thể thay đổi bằng ký tự khác, hoặc dấu cách tùy thuộc vào hiện thị của người sử dụng.

