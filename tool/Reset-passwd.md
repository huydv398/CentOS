# Singer User Mode - Chế độ người dùng đơn
Phôi phục reset password:
1. [CentOS-6.10](#1)
2. [CentOS-7.7](#2)
3. [Ubuntu 1404](#3)
4. [Ubuntu 1604](#4)
5. [Ubuntu 1804](#5)
5. [Ubuntu 2004](#6)

<a name="1"></a>
## 1.Đặt lại / Khôi phục mật khẩu tài khoản người dùng đã quên trong CentOS-6.10

Trong trường hợp quên mật khẩu người dùng, để thực hiện lấy lại mật khẩu root ta làm như sau:
* Khởi động lại máy và và nhấn phím bất kỳ.
* Bạn sẽ được chuyển vào màn hình sau:
![Imgur](https://i.imgur.com/Ve5Fa1K.png)

* Sau khi nhấn phím **e** sẽ chuyển vào kernel OS đang chạy:



* Chọn dòng `kernel /vmlinuz` nhấn **e** :
![Imgur](https://i.imgur.com/e47w7px.png)


* Thêm `1` vào sau `rhgb quite` để chuyển sang chế độ **singer mode** 
![Imgur](https://i.imgur.com/GxGpJUv.png)
![Imgur](https://i.imgur.com/nHeriHV.png)

* Nhấn phím **b** để ***boot***
![Imgur](https://i.imgur.com/8H5syBU.png)


* `passwd root` để lấy lại mật khẩu root
![Imgur](https://i.imgur.com/c99SdOc.png)
* reboot lại máy và nhập mật khẩu mới

<a name="2"></a>
## 2.Đặt lại / Khôi phục mật khẩu tài khoản người dùng đã quên trong CentOS-7.7 1908.

Trong trường hợp quên mật khẩu người dùng, để thực hiện lấy lại mật khẩu root ta làm như sau:
* Khởi động lại máy và và nhấn phím bất kỳ.
* Nhấn **e** bạn sẽ được chuyển vào màn hình sau:
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


<a name="3"></a>

## 3. Đặt lại / Khôi phục mật khẩu tài khoản người dùng đã quên trong Ubuntu 1404
* Khởi động lại máy giữ shift và khi hiện ra màn hình bên dưới
![Imgur](https://i.imgur.com/hSHly50.png)
* Nhấn phím **`e`** , chữ e là viết tắt của edit
* Sau khi nhấn **`e`** bạn sẽ được chuyển vào màn hình sau:
![Imgur](https://i.imgur.com/4fvBnHm.png)

* Tìm đến văn bản: `ro debian-installer/custom-installation=/custom find_preseed=/preseed.cfg auto preseed.file=/floppy/preseed.cfg automatic-ubiquity noprompt` và thay thế nó bằng `rw init=/bin/bash` 


* Sau khi chỉnh sửa xong **ctrl**+**x** hoặc **F10**, nó sẽ bắt đầu khởi động với tham số đã chỉ định. Và sẽ hiện thị **bash prompt** được sử dụng với quyền root.

![Imgur](https://i.imgur.com/1TwvfUD.png)

* Kiểm tra trạng thái của phân vùng gốc bằng lệnh sau:
    * `mount | grep -w /`

![Imgur](https://i.imgur.com/zXkI5ej.png)

* Bây giờ có thể đặt lại mật khẩu cho root
    * `passwd huydv`
![Imgur](https://i.imgur.com/NEre6nZ.png)
* Khởi động lại 
    * `exec /sbin/init`

* Đăng nhập với mật khẩu mới

### Trên là cách reset mật khẩu cho Hệ điều hành Ubuntu-18.04

<a name="4"></a>

## 4.Đặt lại / Khôi phục mật khẩu tài khoản người dùng đã quên trong Ubuntu 1604

* Khởi động lại máy giữ shift và khi hiện ra màn hình bên dưới
![Imgur](https://i.imgur.com/kLKEIgy.png) 
* Nhấn phím **`e`** , chữ e là viết tắt của edit
* Sau khi nhấn **`e`** bạn sẽ được chuyển vào màn hình sau:
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

### Trên là cách reset mật khẩu cho Hệ điều hành và Ubuntu-16.04


<a name="5"></a>

## 5.Đặt lại / Khôi phục mật khẩu tài khoản người dùng đã quên trong Ubuntu 1804
* Khởi động lại máy giữ shift và khi hiện ra màn hình bên dưới
![Imgur](https://i.imgur.com/hSHly50.png)
* Nhấn phím **`e`** , chữ e là viết tắt của edit
* Sau khi nhấn **`e`** bạn sẽ được chuyển vào màn hình sau:
![Imgur](https://i.imgur.com/4fvBnHm.png)

* Tìm đến văn bản: `ro debian-installer/custom-installation=/custom find_preseed=/preseed.cfg auto preseed.file=/floppy/preseed.cfg automatic-ubiquity noprompt` và thay thế nó bằng `rw init=/bin/bash` 


* Sau khi chỉnh sửa xong **ctrl**+**x** hoặc **F10**, nó sẽ bắt đầu khởi động với tham số đã chỉ định. Và sẽ hiện thị **bash prompt** được sử dụng với quyền root.

![Imgur](https://i.imgur.com/1TwvfUD.png)

* Kiểm tra trạng thái của phân vùng gốc bằng lệnh sau:
    * `mount | grep -w /`

![Imgur](https://i.imgur.com/zXkI5ej.png)

* Bây giờ có thể đặt lại mật khẩu cho root
    * `passwd huydv`
![Imgur](https://i.imgur.com/NEre6nZ.png)
* Khởi động lại 
    * `exec /sbin/init`

* Đăng nhập với mật khẩu mới

### Trên là cách reset mật khẩu cho Hệ điều hànhUbuntu-18.04

<a name="6"></a>

## 6.Đặt lại / Khôi phục mật khẩu tài khoản người dùng đã quên trong Ubuntu 2004
* Khởi động lại máy giữ shift và khi hiện ra màn hình bên dưới
![Imgur](https://i.imgur.com/GMVxHXA.png)
* Nhấn phím **`e`** , chữ e là viết tắt của edit
* Sau khi nhấn **`e`** bạn sẽ được chuyển vào màn hình sau:
![Imgur](https://i.imgur.com/TyaqptV.png)

* Tìm đến văn bản: `ro debian-installer/custom-installation=/custom find_preseed=/preseed.cfg auto preseed.file=/floppy/preseed.cfg automatic-ubiquity noprompt` và thay thế nó bằng `rw init=/bin/bash` 


* Sau khi chỉnh sửa xong **ctrl**+**x** hoặc **F10**, nó sẽ bắt đầu khởi động với tham số đã chỉ định. Và sẽ hiện thị **bash prompt** được sử dụng với quyền root.

![Imgur](https://i.imgur.com/eyfIQG5.png)

* Kiểm tra trạng thái của phân vùng gốc bằng lệnh sau:
    * `mount | grep -w /`

![Imgur](https://i.imgur.com/neE3DfW.png)

* Bây giờ có thể đặt lại mật khẩu cho root
    * `adduser nhudahua`

![Imgur](https://i.imgur.com/M55Jt6O.png)

![Imgur](https://i.imgur.com/lEZPeLP.png)

* Khởi động lại 
    * `exec /sbin/init`

* Đăng nhập với mật khẩu mới

### Trên là cách reset mật khẩu cho Hệ điều hành và Ubuntu-20.04

