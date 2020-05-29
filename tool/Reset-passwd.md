# Singer User Mode - Chế độ người dùng đơn
## Đặt lại / Khôi phục mật khẩu tài khoản người dùng đã quên trong CentOS-7.7 1908.

Trong trường hợp quên mật khẩu người dùng, để thực hiện lấy lại mật khẩu root ta làm như sau:
* Khởi động lại máy và và nhấn phím **`e`** , chữ e là viết tắt của edit
* Sau khi nhấn e bạn sẽ được chuyển vào màn hình sau:
![Imgur](https://i.imgur.com/uOMUBZx.png)

* Tìm đến văn bản: `rhgb quite` và thay thế nó bằng `init=/bin/bash` 

* Sau khi chỉnh sửa xong **ctrl**+**x**, nó sẽ bắt đầu khởi động với tham số đã chỉ định. Và sẽ hiện thị **bash prompt**.
![Imgur](https://i.imgur.com/UgNFwEH.png)
* Kiểm tra trạng thái của phân vùng gốc bằng lệnh sau:
    * `mount | grep root`

![Imgur](https://i.imgur.com/lpdQrZE.png)
* Bạn có thể nhận thấy, phân vùng gốc hiển thị `ro`(chỉ đọc). Cần chỉnh sửa quyền đọc ghi trên phân vùng gốc để thay đổi mật khẩu gốc.

```
mount -o remount, rw /
```

![Imgur](https://i.imgur.com/lpdQrZE.png)

* Kiểm tra lại phân vùng:

* Bây giờ có thể đặt lại mật khẩu cho root
    * `passwd root`
    * `touch /.autorelable`

![Imgur](https://i.imgur.com/9RK6yBn.png)

* Khởi động lại 
    * `exec /sbin/init`

* Đăng nhập vào tài khoản root và xem mọi thứ có hoạt động không?

## Đặt lại / Khôi phục mật khẩu tài khoản người dùng đã quên trong Ubuntu 1604
* Khởi động lại máy giữ shift và khi hiện ra màn hình bên dưới
![Imgur](https://i.imgur.com/kLKEIgy.png) 
* Nhấn phím **`e`** , chữ e là viết tắt của edit
* Sau khi nhấn `**e**` bạn sẽ được chuyển vào màn hình sau:
![Imgur](https://i.imgur.com/WOUIVN0.png)

* Tìm đến văn bản: `ro find_preseed=/preseed.cfg noprompt quite` và thay thế nó bằng `rw init=/bin/bash` 
![Imgur](https://i.imgur.com/zCe08Cq.png)

* Sau khi chỉnh sửa xong **ctrl**+**x** hoặc **F10**, nó sẽ bắt đầu khởi động với tham số đã chỉ định. Và sẽ hiện thị **bash prompt**.
![Imgur](https://i.imgur.com/FvcErc3.png)

* Kiểm tra trạng thái của phân vùng gốc bằng lệnh sau:
    * `mount | grep -w /`
    
![Imgur](https://i.imgur.com/jGxkTmW.png)


* Bạn có thể nhận thấy, phân vùng gốc hiển thị `ro`(chỉ đọc). Cần chỉnh sửa quyền đọc ghi trên phân vùng gốc để thay đổi mật khẩu gốc.

```
mount -o remount, rw /
```


* Bây giờ có thể đặt lại mật khẩu cho root
    * `passwd root`

* Khởi động lại 
    * `exec /sbin/init`

* Đăng nhập với mật khẩu mới

