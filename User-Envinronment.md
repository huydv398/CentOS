# User Envinronment (Về người dùng)
1. [User Envinronment](#UserEnvinronment)
2. [Người dùng Root](#nguoidungroot)
3. [Tập tin khởi động](#Startupfile)
3. [Biến môi trường](#environment-var)
5. [Command HISTORY ](#history)
6. [Alias](#alias)

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
Các tài khoản **gốc** có quyền truy cập đầy đủ vào hệ thống.Các hệ điều hành khác thường gọi đây là tài khoản quản trị viên; trong Linux, nó thường được gọi là tài khoản **superuser**.bạn phải cực kỳ thận trọng khi cấp quyền truy cập root đầy đủ cho người dùng; Ví dụ trong các cuộc tấn công bên ngoài thường bao gồm các thủ thuật được sử dụng để nâng lên tài khoản root.Tuy nhiên, bạn có thể sử dụng tính năng sudo để gắn các đặc quyền hạn chế hơn cho các tài khoản người dùng chuẩn:

1. Chỉ trên cơ sở tạm thời
2. chỉ cho một tập con cụ thể của lệnh

Khi gán đặc quyền nâng cao, bạn có thể sử dụng lệnh `su`(chuyển đổi người dùng) để khởi chạy shell mới chạy với tư cách người dùng khác(Bạn phải nhập mật khẩu của người mà bạn muốn trở thành). Thông thường người dùng khác này là root hoặc shell mới cho phép sử dụng các đặc quyền nâng cao cho đến khi thoát.Nó hầu như luôn luôn là một thực hành xấu(nguy hiểm cho cả bảo mật và ổn định) để sử dụng `su` thành root. Lỗi kết quả có thể bao gồm xóa các tệp tin quan trọng khỏi hệ thống và vi phạm bảo mật.
<a name="Startupfile"></a>
## Tập tin khởi động
Trong Linux, chương trình shell lệnh, nói chung bash sử dụng một hoặc nhiều tệp khởi động để cấu hình môi trường. Các tệp trong `/etc` thư mục xác định cài đặt chung cho tất cả người dùng trong khi các tệp khởi tạo trong thư mục chính của người dùng có thể bao gồm và ghi đè chung. Các tệp khởi động có thể làm bất cứ điều gì mà người dùng muốn làm trong mọi lệnh shell, chẳng hạn như:
* Tùy chỉnh lời nhắc của người dùng
* Xác định các phím tắt và bí danh của dòng lệnh
* Đặt trình soạn thảo văn bản mặc định
* Đặt đường dẫn cho nơi tìm chương trình thực thi

Khi bạn đăng nhập lần đầu vào Linux, `/etc/profile` tệp sẽ được đọc và đánh giá, sau đó các lệnh sau được tìm kiếm theo thứ tự được liệt kê:
1. `~/.bash_profile`
2. `~/.bash_login`
3. `~/.profile`

Shell đăng nhập Linux đánh giá bất kỳ tệp khởi động nào mà nó xuất hiện đầu tiên và bỏ qua phần còn lại. Điều này có nghĩa là nếu nó tìm thấy `~/.bash_profile`, nó bỏ qua phần còn lại. Các bạn phân phối khác nhau có thể sử dụng các tệp khởi động khác nhau. Tuy nhiên, Mỗi khi bạn tạo shell mới hoặc cửa sổ termnal,v.v., bạn không thể đăng nhập toàn hệ thống;chỉ có tệp tin `~/.bashrc` được đọc và đánh giá. Mặc dù tệp này không được đọc và đánh giá cùng login shell. Trong các bản phân phối Ubuntu và CenOS, người dùng phải thực hiện các thay đổi phù hợp trong tệp ` ~/.bash_profile` để bao gồm tệp `~/.bashrc`. Các tệp `~/.bash_profile` sẽ có một số dòng thêm, do đó sẽ thu thập thông số tùy yêu cầu từ `~/.bashrc`.

<a name="environment-var"></a>
## Biến môi trường
Các biến môi trường được đặt tên đơn giản là các đại lượng có giá trị cụ thể và được hiểu bởi lệnh shell, chẳng hạn như **bash**. Một số trong số này được hệ thống cài đặt sẵn và một số khác được người dùng đặt ở dòng lệnh hoặc khi khởi động và các tệp lệnh khác. Một biến môi trường thực sự không nhiều hơn một chuỗi ký tự chứa thông tin được sử dụng bởi một hoặc nhiều ứng dụng. Có một số các để xem các giá trị của các biến môi trường hiện được đặt. Tất cả các lệnh `set`,`env` và `export` hiển thị các biến môi trường.

Theo mặc định, các biến được tạo trong một tệp lệnh chỉ có sẵn cho chương trình bao hiện tại. Tất cả các tiến trình con(shell phụ) sẽ không có quyền truy cập vòa các giá trị đã được đặt hoặc sửa đổi. Cho phép các tiến trình con xem các giá trị, Yêu cầu sử dụng lệnh Export.

|task|command|
|----|-------|
|hiển thị giá trị của một biến cụ thể|`echo $SHELL`
|Xuất một biến mới|export VAR=value|
|Thêm một biến vĩnh viễn|Add the line export VAR=value to ~/.bashrc|
**HOME** là một biến môi trường đại diện cho HOME hoặc đăng nhập thư mục của người dùng.Các lệnh `cd` có đối số sẽ thay đổi thư mục làm việc hiện tại với giá trị của HOME. Lưu ý ký tự dấu ngã(~) Thường được sử dụng làm chữ viết tắt cho $HOME.

Biến môi trường**PATH**, là một danh sách có thứ tự thư mục sẽ được quế khi một lệnh được đưa ra để tìm các chương trình hay kịch bản thích hợp để chạy. Mỗi thư mục trong đường dẫn được phân tách bằng dấu hai chấm(:). Tên thư mục trống cho biết thư mục hiện tại tại bất kỳ thời điểm nào.

Các biến môi trường **PS** được sử đụng để tùy chỉnh chuỗi dấu nhác của bạn trong cửa sổ terminal của bạn đẻ hiển thị các thông tin mà bạn muốn.**PS1** là biến dấu nhắc chính điều khiển dấu nhắc dòng lệnh của bạn trông như thế nào. Các ký tự đặc biệt sau đây có thể được bao gồm trong **PS1**:

|Ký tự|Sử dụng|
|-|-|
|\u|Tên tài khoản|
|\h|Tên máy chủ|
|\W|Thư mục làm việc hiện tại|
|\ !| Số lịch sử dòng lệnh|
|\d|Ngày  |

Chúng phải được bao quanh trong dấu ngoặc đơn khi chúng được sử dụng
```
[root@server ~]# man export
[root@server ~]# echo $PS1
[\u@\h \W]\$
[root@server ~]# export PS1='\d-\u@\h:\w$'
Mon Apr 27-root@server:~$
Mon Apr 27-root@server:~$
```
Chúng ta có thể nhìn thấy sự khác biệt khi `export` thêm \d vào biến *PS1*
Các điểm của biến môi trường **SHELL** sử dụng shell command mặc định ( các chương trình được xử lý bất cứ điều gì bạn gõ vào một cửa sổ lệnh, thường **bash**) và chứa các đường dẫn đầy đủ đến shell.
```
[root@server ~]# echo $SHELL
/bin/bash
```
<a name="history"></a>
## Command history
Bash theo dõi các lệnh và câu lệnh đã nhập trước đó trong bộ đệm lịch sử dụng trước đó bằng cách sử dụng các phím con trỏ Lên và Xuống. Dể xem danh sách các lệnh đã thực hiện trước đó, bạn cũng có thể sử dụng lệnh `HISTORY ` . Danh sách các lệnh được hiển thị với lệnh gananf đay nhất xuất hiện cuối cùng trong danh sách. Thông tin nàu được lưu trữ trong tập tin `~/.bash_history `.
```
[root@server ~]# history
    1  ip a
    2  ping google.com
...
  148  mkdir huy12
  149  chown -R huy:basic /home/huy1
  150  chown -R huy1:basic /home/huy1
  151  chown -R huy2:basic /home/huy2
...
  365  export PS1='[\u@\h \W]\ $'
  366  echo $SHELL
  367  history
[root@server ~]#
```

 Một số biến môi trường liên quan có thể sử dụng để lấy thông tin về tệp lịch sử.

 |Variable |Được sử dụng|
 |--|-|
 |HISTFILE |Biến `$HISTFILE` được trỏ đến tệp lịch sử của bạn
 |HISTFILESIZE |Số lượng lệnh được lưu trữ treong tệp lệnh của bạn
 HISTSIZE|In ra số lượng lệnh sẽ được ghi nhớ trong môi trường của bạn
 ```
 [root@server ~]# echo $HISTSIZE
1000
[root@server ~]# echo $HISTFILE
/root/.bash_history
[root@server ~]# echo $HISTFILESIZE
1000
```
Bảng dưới đây cho thấy cú pháp được sử dụng để thực hiện các lệnh được sử dụng để thực hiện các lệnh trước đó
|Syntax|Được sử dụng|
|-|-|
|!!|Thực hiện lệnh trước đó|
|!|Bắt đầu thay thế lịch sử|
|!$|Tham khảo đối số của cùng của một dòng|
|!n|Tham khảo dòng lệnh thế n|
|!string|Tham khảo lệnh gần đây nhất bắt đầu bằng string|

<a name="alias"></a>
## Aliases
Các lệnh tùy chỉnh có thể được tạo ra để sửa đổi hành vi của những cái dã có bằng cách tạo Alias.Thông thường các bí danh này được đặt trong tệp `~/.bashrc`của bạn để chúng có sẵn cho bất kỳ shell lệnh nào bạn tạo. Các lệnh `Aliases` với không có đối số sẽ liệt kê cac sbis danh quy định hiện hành
```
[root@server ~]# alias
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
[root@server ~]#

```
Để tạo một phím tắt Alias ta có :
`alias [new_command]=[command]`

`[root@server ~]# alias c='clear'
`
Bây giờ chỉ cần gõ` c` thay vì gõ `clear`