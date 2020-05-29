# GREP
* Là lệnh tìm kiếm văn bản trong Linux.
* Grep là tên viết tắt của "Global Regular Expression Print". Grep có thể sử dụng để xe dữ liệu đầu vào mà nó nhận được có khớp với mẫu không.

## Cách khi dụng cơ bản
* Grep có thể được sử dụng để khớp với các mẫu chữ trong tệp văn bản

Ví dụ có 2 file văn bản sau:
```
[root@server1 file]# cat WP
Tim hieu wordpress
Cai dat wordpress
cai dat DB
Su dung Database
Apache la gi
[root@server1 file]# cat VMware
VMware la gi
Cai dat VMware
cai dat database
Su dung Netnork
SSH bang MobaXterm
Cai wordpress
```

### 1.Tìm một chuỗi trong file
VD: Tìm từ **"cai dat"** trên file Wp

![Imgur](https://i.imgur.com/it86KcW.png)

### 2.Tìm kiếm chuỗi trên nhiều file
VD: Tìm từ **"Su dung"** trên 2 file WP và VMware

![Imgur](https://i.imgur.com/WEZfnH3.png)

### 3. Tìm kiếm không phân biệt chữ hoa chữ thường sử dụng tùy chọn `-i`
VD: Trên 2 File có cả từ viết thường và viết hoa. Để tìm kiếm mà không phân biệt hoa thường sử dụng tùy chọn `-i`:

![Imgur](https://i.imgur.com/5H5raNR.png)

Ở đây có thể nhìn thấy sự khác biệt rõ ràng giữa sử dụng `-i` và không sử dụng.

### 4. Tìm kiếm ngược, sử dụng tùy chọn -v
VD: Để tìm tất cả các dòng không chứa chữa chữ "cai"

![Imgur](https://i.imgur.com/nm2Slbn.png)

Kết quả sẽ hiện thị những dòng không chứa từ **cai**

### 5. Hiển thị số dòng, số lượng, giới hạn số dòng đầu ra 
* -n : Hiển thị số thứ tự của dòng và dòng chứa từ cần tìm

![Imgur](https://i.imgur.com/JPV0sGF.png)

     Hiện thị số dòng mà key cần tìm nằm tại dòng thứ 5
* -c : Đếm số dòng khớp với kí tự cần tìm

![Imgur](https://i.imgur.com/LQyph2f.png)
Có 1 Key phù hợp

![Imgur](https://i.imgur.com/RJIasik.png)
Có 2 từ phù hợp

![Imgur](https://i.imgur.com/nYfFGfu.png)
Có 4 từ phù hợp ký tự cần tìm

* -m: Giới hạn số lượng dòng khớp.
![Imgur](https://i.imgur.com/jFw6I95.png)

  `-m3`: chỉ cho phép hiện thị 3 tìm kiếm.

### 6.Tìm kiếm nhiều chuỗi

Có 3 cú pháp tìm tương đương nhau:
* Cách 1: `grep -e "keyword1" -e "keyword2" [file]`
* Cách 2: `grep "word1\|word2" [file]` # \ để phân biệt word1 với word2
* Cách 3: `egrep "word1|word2" [file]`

```
[root@server1 ~]# grep 'huy2\|huy5\|root' /etc/passwd
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
huy2:x:1001:1001::/home/huy2:/bin/bash
huy5:x:1004:1004::/home/huy5:/bin/bash
```

### 7.Tìm kiếm tên tệp
* `-l` : Để có được các tập tin phù hợp với tìm kiếm
* `-L` : Để có được các tập tin không phù hợp với tìm kiếm
* `-h` : hiển thị không tên file ( khi chỉ định nhiều file)
* `-H` : Hiển thị cùng với tên file

#### 7.1 So sánh giữa 2 tùy chọn `-l` và `-L`
![Imgur](https://i.imgur.com/g9Z5LyC.png)
* *Tùy chọn `-l` Tìm kiếm trong thư mục **file**, In ra các tên file bên trong có chứa ký tự `cai`

![Imgur](https://i.imgur.com/XKMcykr.png)
* *Tùy chọn `-L` sẽ ra kết quả là những tên file không có chứa từ* **cai**
#### 7.2 So sánh 2 tùy chọn `-h` và `-H`
![Imgur](https://i.imgur.com/HpX8MTA.png)

* Tùy chon `-h` sẽ cho kết quả những dòng chứa từ **"cai dai"** mà không kèm với tên file

* Còn với tùy chọn `-H` kết quả sẽ bao gồm tên file và dòng chứa từ **"cai dat"**
### 8. Hiển thị thêm dòng trước, sau, xung quanh dòng chứa kết quả cần tìm.
`grep -<A, B hoặc C> <n> "chuỗi" [file]`

Trong đó:
* `A` : Hiển thị dòng khớp với ký tự được tìm và dòng dưới nó.
* `B` : Hiển thị dòng khớp với ký tự được tìm và dòng trên của nó.
* `C` : Hiện thị dòng xung quanh dòng khớp với kí tự cần tìm.Vidu C2 thì hiện thị 2 dòng trên và 2 dòng dưới của dòng được khớp.
* `n` : Là số tự nhiên chỉ định xem hiển thị trước, sau hay xunh quanh bao nhiêu dòng
![Imgur](https://i.imgur.com/Tq60H7P.png)
### 9. Tìm chính xác với `-w`
VD: Mình có file info
![Imgur](https://i.imgur.com/HF1uXjm.png)

* Khi bạn tìm kiếm bình thường với `grep`, kết quả sẽ hiển thị tất cả những dòng có chứa từ >>> kể cả >>>>>.
* Với tùy chọn `-w`, kết quả sẽ tìm chính xác chỉ những dòng có chứa từ >>>/

### 10 Đọc file loại bỏ comment, dòng trống

![Imgur](https://i.imgur.com/KeeNrFj.png)
Đây là file có nhiều comment

![Imgur](https://i.imgur.com/PxXbjno.png)
Và đây là file sau khi đã loại bỏ các comment