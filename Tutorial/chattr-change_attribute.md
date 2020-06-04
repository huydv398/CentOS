# Change attribute
* `chattr là câu lệnh cho phép bạn thay đổi thuộc tính của file giúp bảo vệ file khỏi bị xóa hoặc ghi đè nội dung, dù bạn có là user root

Cú pháp câu lệnh:

`chattr [operator] [flat] [filename]`

Operator:
* `+` : thêm thuộc tính cho file
* `-` : gỡ bỏ thuộc tính khỏi file
* `=` : giữ nguyên thuộc tính của file

Các flag (hay các thuộc tính của file):

Các flag thường dùng là:
* `R`- Recursively: Thay đổi đệ quy các thuộc tính của thư mục và các cấu hình của chúng.
* `i`-(immutable): Flag này khiến file không thể rename, Không thể tạo symlink, Không thể thực thi, không thiể write. Chỉ có user rôt mới set và uset được thuộc tính này.
* `a`: Khiến file không thể : rename, tạo symlink, không thể thực thi, chỉ có thể thêm nội dung vào file. Chỉ root mới có thể set và unset được thuộc tính này.


* `d` : file có thuộc tính này sẽ không được backup khi tiến trình dump chạy.
* `s` : Nếu file có thuộc tính này bị sửa, thay đổi sẽ đọc cập nhật đồng bộ hóa trên ổ cứng.
* `A` : khi File có thuộc tính được truy cập, giá trị `atime` của file sẽ không thay đổi.

**Lưu ý**: Tất cả các lệnh dưới đây đều chạy dưới quyền user root

Tạo một file trong thư mục
## Cách thêm thuộc tính trên file để bảo vệ file khỏi bị xóa
Chúng ta thêm thuộc tính `i` (*immutable*)- không thể rename, Không thể tạo symlink, Không thể thực thi, không thể write, không thể xóa.

```
# Thực hiện thêm thuộc tính 
[root@server1 duonghuy]# chattr +i huyts.txt

#Lệnh check hiệu lực
[root@server1 duonghuy]# lsattr
----i----------- ./huyts.txt

#Thực hiện xóa file
[root@server1 duonghuy]# rm -rf huyts.txt
rm: cannot remove ‘huyts.txt’: Operation not permitted

[root@server1 duonghuy]# mv huyts.txt file.txt
mv: cannot move ‘huyts.txt’ to ‘file.txt’: Operation not permitted

[root@server1 duonghuy]# chmod 755 huyts.txt
chmod: changing permissions of ‘huyts.txt’: Operation not permitted

```
Có thể thấy rằng không thể thay đổi file.


## Cách để unset thuộc tính đã thêm cho file
Ta sử dụng operator `-`
Unset thuộc tính `i` trên file `huyts.txt`

```
# Unset thuộc tính `i` trên file huyts.txt
[root@server1 duonghuy]# chattr -i huyts.txt

# Kiểm tra bằng cách thay đổi
[root@server1 duonghuy]# lsattr
---------------- ./huyts.txt

[root@server1 duonghuy]# mv huyts.txt huyfile.txt
[root@server1 duonghuy]# ll
total 4
-rw-rw-r--. 1 duonghuy duonghuy 154 Jun  3 14:08 huyfile.txt

```

## Chỉ cho phép nối thêm vào nội dung file
```
#Thêm thuộc tính `a`(append) cho file
[root@server1 duonghuy]# chattr +a huyfile.txt
[root@server1 duonghuy]# lsattr
-----a---------- ./huyfile.txt

#Thử sửa nội dung file
[root@server1 duonghuy]#  echo "sua noi dung file" > huyfile.txt
-bash: huyfile.txt: Operation not permitted

#Chèn nội dung file
[root@server1 duonghuy]#  echo "Chen noi dung file" >> huyfile.txt
[root@server1 duonghuy]# cat huyfile.txt
Duong van Huy
4444

Chen noi dung file
[root@server1 duonghuy]#

#Ta có thể gỡ thuộc tính này:
[root@server1 duonghuy]#  chattr -a huyfile.txt

```
**Kết luận**: `-a` có thể chỉ chèn thêm nội dung  
## Cách dùng chattr để bảo vệ thư mục

Để bảo vệ cả thư mục và các file bên trong thư mục đó
dùng `-R` (recursively) và `+i` với đường dẫn của thư mục đó.

```
# Thêm thuộc tính cho thư mục
[root@server1 home]# chattr -R +i duonghuy/

#Xóa thư mục
[root@server1 home]# rm -rf duonghuy/
rm: cannot remove ‘duonghuy/.bash_logout’: Permission denied
rm: cannot remove ‘duonghuy/.bash_profile’: Permission denied
rm: cannot remove ‘duonghuy/.bashrc’: Permission denied
rm: cannot remove ‘duonghuy/.bash_history’: Permission denied
rm: cannot remove ‘duonghuy/huyfile.txt’: Permission denied

#unset quyền trên.
[root@server1 home]# chattr -R -i duonghuy/

```
## Áp dụng để bảo vệ file /etc/passwd và file /etc/shadow

Thêm thuộc tính `i` cho các file này để tránh bị xóa nhầm. Lưu ý rằng ta không thể tạo thêm user trên khi sử dụng cách này.

```
[root@server1 ~]# chattr +i /etc/passwd

#Thêm User.
[root@server1 ~]# useradd nhanhoa
useradd: cannot open /etc/passwd

```
Không thể thêm vào file `/etc/passwd` do file đã thêm thuộc tính *immutable* nên file không thể chỉnh hoặc thêm

## Kết luận 
Trên là bài lab về phần
Bảo vệ tệp tin cho thư mục và file bằng câu lệnh `chattr`
