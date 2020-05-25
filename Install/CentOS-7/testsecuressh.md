# Các trường hợp có thể sảy ra với SSH và xem cảnh báo log
* Khi SSH thành công với key **private**:<br>![Imgur](https://i.imgur.com/jVbDW5m.png)
    * `Accepted`: Đươc chấp nhận publickey từ người dùng root và từ ip 192.168.20.1 port 61794 và mã SRA
    * `pam_unix`: phiên của seeson đang được mở

* Khi SSH đúng **Key private** nhưng sai user<br>![Imgur](https://i.imgur.com/1HpvUJ7.png)
    *  `Invalid` :người dùng không hợp lệ
    * `huyhuy`: user đã đăng nhập
    * `ip` là 192.168.20.1
    * `Lỗi` : ngắt kết nối
* Khi SSH đúng ***pritekey*** sai **passphrase**
![Imgur](https://i.imgur.com/IeM02f4.png)
    * Đóng kết nối
* Khi SSH đúng **privatekey** từ máy client và nhập đúng **passphrase**<br>![Imgur](https://i.imgur.com/taFx2Rg.png)
    * Đăng nhập thành công từ root và ip là 20.2
    * seeson đã đã được mở kết nối
* Khi SSH bằng Moba không có **Privatekey**
![Imgur](https://i.imgur.com/BIRpksU.png)
* Đã lưu **private** trong *client* và **SSH** sai mật khẩu
![Imgur](https://i.imgur.com/JGyjHyS.png)