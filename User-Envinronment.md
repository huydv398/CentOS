# User Envinronment (Về người dùng)
1. [User Envinronment](#UserEnvinronment)
2. [Người dùng Root](#nguoidungroot)

Linux là một hệ điều hành người dùng, nơi có nhiều hơn một người dùng có thể đăng nhập cùng một lúc. Các danh sách lệnh ``who`` hiện đang đăng nhập. Để xác định người dùng hiện tại, sử dụng lệnh ``whoami``.

``` 
[root@server ~]# who
huy1     tty1         2020-04-27 15:31
root     pts/0        2020-04-27 15:31 (192.168.65.1)
```
Linux sử dụng các nhóm để tổ chức người dùng. Nhóm là tập hợp các tài khoản với các quyền được chia sẻ nhất định. Kiểm soát thành viên nhóm và thành viên nhóm được quản lý thông qua tệp `/etc/group`, Trong đó hiện thị danh sách các nhóm và thành viên của họ.Theo mặc định, mọi người thuộc về một nhóm mặc định hoặc nhóm chính. Khi người dùng đăng nhập, thành viên nhóm được đặt cho nhóm chính của họ và tất cả thành viên được hưởng cùng mức truy cập và đặc quyền của nhóm đó. Quyền trên các tệp tin và thư mục khác nhau có thể sửa đổi ở cấp độ nhóm.

Tất cả người dùng Linux được gắn một ID người dùng duy nhất,**uid**, Chỉ là một số nguyên, cũng như một hoặc nhiều ID nhóm, **gid**, Bao gồm một ID mặc định với ID người dùng. Trong lịch sử, các bản phân phối dựa trên HatRed bắt đầu từ 500. Các bản phân phối khác bắt đầu từ 1000. Các số này được liên kết với tên thông qua các tệp `/etc/passwd/` và `/etc/group`. Các nhóm được sử dụng để thiết lập một nhóm người dùng có lợi ích chung cho các mục đích về quyền truy cập, đặc quyền và cân nhăc bảo mật. Quyền truy cập vao tệp và thiết bị được cấp trên cơ sở người dùng và nhóm mà họ thuộc về.

Chỉ người dung root mới có thể thêm và xóa  người dùng và nhóm. Thêm một người dùng mới được thực hiện bằng lệnh `adduser` và loại bỏ một người dùng bằng lệnh `userdel`. Ở dạng đơn giản nhất, một tài khoản cho người dùng mới **``userdemo``** sẽ được thực hiện với

```
[root@server ~]# useradd userdemo
[root@server ~]# cat /etc/passwd | grep userdemo
userdemo:x:1005:1007::/home/userdemo:/bin/bash
```
Theo mặc định, thư mục chính và một số tệp lệnh cơ bản của `userdemo` là `/home/userdemo ` và đặt shell mặc định thành `/bin/bash`
```
[root@server ~]# ls -la /home/userdemo
total 12
drwx------.  2 userdemo userdemo  62 Apr 27 15:54 .
drwxr-xr-x. 11 root     root     167 Apr 27 15:54 ..
-rw-r--r--.  1 userdemo userdemo  18 Aug  8  2019 .bash_logout
-rw-r--r--.  1 userdemo userdemo 193 Aug  8  2019 .bash_profile
-rw-r--r--.  1 userdemo userdemo 231 Aug  8  2019 .bashrc
```

Xóa tài khoản người dùng bằng cách gõ:

```
[root@server ~]# userdel userdemo
[root@server ~]# cat /etc/passwd | grep userdemo
[root@server ~]# ls -la /home/userdemo
total 12
drwx------.  2 1005 1007  62 Apr 27 15:54 .
drwxr-xr-x. 11 root root 167 Apr 27 15:54 ..
-rw-r--r--.  1 1005 1007  18 Aug  8  2019 .bash_logout
-rw-r--r--.  1 1005 1007 193 Aug  8  2019 .bash_profile
-rw-r--r--.  1 1005 1007 231 Aug  8  2019 .bashrc
```
Tuy nhiên, điều này sẽ để lại thư mục `/home` nguyên vẹn. Điều này có thể hữu ích nếu nó là tạm thời hoãn hoạt động. Để xóa thư mục chính trong khi xóa tài khoản, người ta cần sử dụng tùy chọn liên quan.
```
[root@server ~]# useradd huydemo
[root@server ~]# userdel -r huydemo
[root@server ~]# cat /etc/passwd | grep huydemo
[root@server ~]# ls -la /home/huydemo
ls: cannot access /home/huydemo: No such file or directory
```

lệnh `id` Không có đối số cung cấp thông tin về người dùng hiện tại. Nết được đặt tên của người dùng khác làm đối số, id sẽ báo cáo thông tin về người dùng khác đó.
Đối với người dùng là root
```
[root@server ~]# id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```

Đối với người dùng là user:
```
[huy1@server ~]$ id
uid=1002(huy1) gid=1005(basic) groups=1005(basic) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```
Sử dụng lệnh passwd ddeeer thay đổi mật khẩu cho người dùng mới:
```
[root@server ~]# useradd usertest
[root@server ~]# passwd usertest
Changing password for user usertest.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
```
Thêm một nhóm mới được thực hiện với lệnh `groupadd`và loại bỏ bằng lệnh `groupdel`.
```
[root@server ~]# groupadd new1group
[root@server ~]# groupdel new1group
```

Thêm người dùng vào một nhóm đã tồn tại được thực hiện bằng lệnh `usermod`.
```
[root@server ~]# groupadd newgroup
[root@server ~]# usermod -G newgroup huy1
[root@server ~]# groups huy1
huy1 : basic newgroup
```
Cú pháp:
* `usermod [option] [name_group] [name_user]`
   * option:
     * `-g` : Thêm user vào một group
     * `-G` : Thêm user vào nhiều group(mỗi gruop được phân tách group tiếp theo bằng dấu phẩy,không có sự can thiệp từ khoảng trắng)

Tất cả các tệp này cập nhật khi cần thiết tại thư mục `/etc/group`. Các lệnh ` group` có thể được sử dụng để thay đổi các nhóm thuộc nhóm như Nhóm ID hoặc tên
```
[root@server ~]# groupmod newgroup -n grouponlyread
[root@server ~]# groups huy1
huy1 : basic grouponlyread
```
<a name="nguoidungroot"></a>
## Người dùng root