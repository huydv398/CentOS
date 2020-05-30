# Basic Telegram
* Cảnh báo Telegram_bot là khi có người dùng SSH vào máy thì sẽ có cảnh báo về Telegram_bot.
* Các bước cài: 
    * Tạo Telegram_bot.
    * Gửi cảnh báo tới Telegram qua API Token.

### Tạo Telegram_bot

* Tìm kiếm @BotFather.

* Chat "/start".

* Chat "/**newbot**": Tạo 1 bot mới.

* Tạo tên cho Bot, Chat tên bot muốn tạo. Ví dụ:

    * ![Imgur](https://i.imgur.com/nDMSHYt.png)

    Ví dụ mình đặt tên cho bot là huytest_bot
* Tạo tên username cho bot, Nó phải kết thúc bằng **bot**
    * ![Imgur](https://i.imgur.com/HwzYunp.png)
* Tạo thành công tìm tại thanh tìm kiếm tên bot hoặc tên Username của boot:
    * ![Imgur](https://i.imgur.com/fXDvfJi.png)

#### Tạo group để lấy chat_id
##### Cách1
![Imgur](https://i.imgur.com/Yghl5AU.png)

* Add Members vào nhóm: @MissRose_bot 

* `/id @user` 
    * Rose sẽ trả lời cho bạn biết ID của bạn là gì

    ![Imgur](https://i.imgur.com/ifij6Ae.png)

Cách 2:
* Chat với Telegram_bot vừa tạo.
* https://api.telegram.org/bot[Token]/getUpdates

![Imgur](https://i.imgur.com/qe0gdh8.png)

* Khi đó trình duyệt sẽ hiện tất cả các thông báo trong đó có username, ID, văn bản được gửi, ...

Tạo xong Telegram_bot

### Gửi cảnh báo tới Telegram qua API Token

- Chuẩn bị :

* 2 máy linux có IP Public (hoặc IP Private) có kết nối ra bên ngoài Internet.
* Thao tác với user `root`

<h4>Cài đặt

* `jq` là ứng dụng để đọc thông tin file JSON trên Linux.

Trên Ubuntu:
* `apt-get -y install jq`

Trên CentOS-7:
* `yum install epel-release -y` 
* `yum install -y jq`

* Tạo file Script:
    * Tại thư mục `/etc/profile.d/`. Để khi đăng nhập vào hệ thống thì script sẽ thực hiện ngay lập tức.
    * Tạo file script `ssh-telegram.sh`:

    ` vi /etc/profile.d/ssh-telegram.sh`

    * Nội dung script: https://news.cloud365.vn/script-3-giam-sat-truy-cap-ssh-vao-he-thong/
    
    * Cấp quyền thực thi cho thư mục:

    `chmod +x /etc/profile.d/ssh-telegram.sh`

* Lưu ý :UserID và Token sẽ là ID Chat và API Token của bot telegram của bạn.

### Kiểm tra 
Tiến hành SSH và Server. Ta sẽ thay cảnh báo từ Telegram như sau:
![Imgur](https://i.imgur.com/MjIHOvP.png)