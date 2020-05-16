# Cách cài đặt Mount NFS trên Ubuntu 20.04
[Giới thiệu Network Lile System](#intro)
1. [Tải xuống và cài đặt các thành phần](#down) 
2. [Tạo thư mục trên máy chủ](#cre)
3. [Cài đặt cấu hình Export NFS trên máy chủ lưu trữ](#conf)
4. [Điều chỉnh tường lửa trên máy chủ](#ufw)
5. [Tạo Mount và thư mục gắn kết trên máy Client](#5)
6. [Kiểm tra truy cập NFS](#6)
7. [Gắn thư mục Mount khi khởi động](#7)
8. [Ngắt kết nối chia sẻ Mount (Umount)](#8)
9. [Kết luận](#end)
    
## Giới thiệu 
 <a name="intro"></a>
NFS là giao thức hệ thống tệp phân tán cho phép bạn gắn các thư mục từ xa trên máy chủ của mình. Điều này cho phép bạn quản lý không gian lưu trữ ở một vị trí khác và ghi vào không gian đó từ nhiều khách hàng. NFS cung cấp một cách tương đối chuẩn và hiệu quả để truy cập hệ thống từ xa qua mạng và hoạt động tốt trong các tình huống mà các tài nguyên được chia sẻ phải được truy cập thường xuyên.

Trong hướng dẫn này, tôi sẽ giới thiệu các cài đặt các phần mềm cần thiết cho chức năng NFS trên Ubuntu 20.04, Dịnh cấu hình hai giá trị NFS trên máy Server và máy Client, Muont và Umuont các thiết bị từ xa.

<a name="b1"></a>
## 1) Tải xuống và cài đặt các thành phần
<a name="down"></a>
Ta sẽ cần hai máy chủ Ubuntu 20.04.

### Trên máy chủ **SERVER**
Cài đặt Gói `nfs-kernel-server`, cho phép bạn chia sẻ các thư mục của mình. Vì đây là thao tác đầu tiên bạn thực hiện `apt`, hãy update các goiscuar bạn trước khi cài đặt:<br> `sudo apt update` <br> `sudo apt install nfs-kernel-server` <br> Khi các gói này được cài đặt, chuyển sang máy **Client**
### Trên máy **CLIENT**
Trên server Client, Tôi cần phải cài đặt gói phần mềm được gọi là `nfs-common`, cung cấp chức năng NS mà không bao gồm bất kỳ thành phần máy chủ.Và Update lại gói:<br> `sudo apt update` <br> `sudo apt install nfs-common` <br> 
Bây giờ cả hai máy đều có các gói cần thiết, Tôi có thể bắt đầu cấu hình chúng.

<a name="b2="></a>
## 2) Tạo thư mục chia sẻ trên máy chủ
<a name="cre"></a>
Tôi sẽ chia sẻ hai thư mục riêng biệt, với các cài đặt cấu hình khác nhau, để minh họa hai cách chính mà các Mount NFS có thể được cấu hình lien quan đến truy cập của người dùng.

Người dùng có thể làm bất cứ điều gì trên hệ thống của họ. Tuy nhiên, các thư mục gắn trên NFS không phải là một phần của hệ thống mà chúng được gắn kết,do đó, theo mặc định máy chủ NFS từ chối thực hiện các hoạt động yêu cầu đặc quyền người dùng. Hạn chế mặc định này có nghĩa là các người dùng trên máy khách không thể ghi các tệp dưới dạng root, gán lại quyền sở hữu  hoặc thực hiện bất kỳ tác vụ người dùng nào khác trên NFS.

Tuy nhiên, có những người dùng đáng tin cậy trên hệ thống **Client**, những người cần thực hiện các hành động này trên hệ thống tệp được Muont nhưng không có nhu cầu truy cập người dùng trên máy **Server**. Bạn có thể định cấu hình máy chủ NFS để cho phép điều này, mặc dù nó mở đầu một yếu tố rủi ro, vì như vậy người dùng có thể có quyền truy cập root vào toàn bộ hệ thống máy Server.

#### Ví dụ 1: Xuất ra các Mount với mục đích chung
Ví dụ này, ta tạo ra một Mount NFS đa năng sử dụng hành vi NFS mặc đinh để gây khó khăn cho người dùng có quyền root trên trên máy Client để tương tác với các máy **Server** bằng các đặc quyền siêu người dùng của máy **Client**. Bạn có thể sử dụng để lưu trữ các tệp được tải lên bằng hệ thống quản ly nội dung hoặc tạo không gian cho người dùng dễ dàng chia sẻ tệp dự án.

* Tạo mục chia sẻ:<br> `sudo mkdir /var/nfs/general -p`
* Vì Tôi tạo nó với sudo, thư mục được sở hữu bởi người dùng root của máy chủ lưu trữ:<br>`ls -la /var/nfs/general`
![Imgur](https://i.imgur.com/kAK6phN.png)

NFS sẽ phiên dịch các hoạt động nào của **root** trên **client** đến `nobody:nogroup` thông tin đăng nhập như một biện pháp bảo mật. Do đó, ta cần thay đổi quyền sở hữu thư mục để phù hợp với các thông tin đăng nhập.<br> `huydv@hsv:~$ sudo chown nobody:nogroup /var/nfs/general`

#### Xuất ra thư mục chính(Home Directory)
Trong ví dụ này, mục tiêu là làm cho các thư mục `home` của người dùng được lưu trữ trên máy **Server** có sẵn trên máy **Client**, đồng thời cho phép các quản trị viên đáng tin cậy của các máy **Server-Client** truy cập cập mà họ cần để quản lý người dùng một cách thuận tiện.

Để làm điều này, ta sẽ Export thư mục `/home`. Vì nó đã tồn tại, ta không cần phải tạo ra nó. Ta cũng sẽ không thay đổi quyền. Nếu ta làm như vậy, nó có thể dẫn đến một loại các vấn đề cho bất kỳ ai có thư mục /home trên máy **Server**.

## 3)Cài đặt cấu hình Export NFS trên máy chủ lưu trữ(**SERVER**) 
<a name="conf"></a>
Tiếp theo sẽ đi sâu vào tệp cấu hình NFS để thiết lập việc chia sẻ các tài nguyên này.

Trên máy **SERVER**, Mở tệp `/etc/export ` trong trình soạn thảo văn bản của bạn với quyền `root`:<br>`huydv@hsv:~$ vi /etc/exports`

* Cú pháp của từng dòng:
```
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)

#directory_to_share    client(share_option1,...,share_optionN)
```
* Trong đó:
    * `Directory_to_share`: Nguồn thư mục tại máy server
    * `client` : nguồn thư mục tại máy client

Ta cần tạo một dòng cho thư mục mà tôi dự định chia sẻ. Hãy thay đổi `client_ip` thành địa chỉ IP thực tế của bạn:
```
/var/nfs/general    client_ip(rw,sync,no_subtree_check)
/home               client_ip(rw,sync,no_root_squash,no_subtree_check)`
```
![Imgur](https://i.imgur.com/l5cejWk.png)

Ở đây, tôi sử dụng các tùy chọn cấu hình giống nhau cho cả hai thư mục ngoại trừ `no_root_squash`.Đây là ý nghĩa của từng Option trong câu lệnh:
* share option:
    * `rw`: Tùy chọn này cung cấp cho **Client** cả quyền truy cập đọc và ghi vào ổ đĩa.
    * `sync`: Tùy chọn này buộc NFS ghi các thay đổi vào đĩa trước khi trả lời. Điều này dẫn đến một môi trường ổn định và nhất quán hơn vì phản hồi phản ánh trạng thái thục tứ của khối từ xa. Tuy nhiên, nó cũng làm giảm tốc độ hoạt động của tập tin.
    * `no_subtree_check`: Tùy chọn này ngăn kiểm tra subtree, một quá trình trong đó máy **Server** phải kiểm tra xem các tệp có thực sự vẫn có sẵn trong cây được được xuất cho mỗi yêu cầu hay không. Điều này có thể gây ra nhiều vẫn đề khi một tệp được đổi tên trong khi máy **Client** đã mở. Trong hầu hết các trường hợp nên vô hiệu hóa subtree. 
    * `no_root_squash`: Theo mặc định, NFS chuyển các yêu cầu từ người dùng **root** từ xa sang người dùng không có đặc quyền trên máy chủ. Điều này được dự định là tính năng bảo mật để ngăn tài khoản **Root** trên máy **Client** sử dụng hệ thống tệp tin của máy **Server** làm **root**

Khi bạn hoàn thành việc thay đổi, hãy lưu và đóng tệp. Sau đó, để cung cấp các chia sẻ cho các máy Client mà bạn đã cấu hình, Hãy khởi động lại dịch vụ máy chủ NFS bằng câu lệnh sau:<br>`root@hsv:~# systemctl restart nfs-kernel-server`

Tuy nhiên, trước khi bạn thực sự có thể sử dụng các chia sẻ mới, bạn sẽ cần chắc chăn lưu lượng truy cập vào các shared được cho phép theo tuy tắc tường lửa.

## 4) Điều chỉnh tường lửa trên máy chủ
<a name="ufw"></a>
Trước tiên, hãy kiểm tra trạng thái tường lửa để xem nó có được bật không và nếu có, để xem những gì hiện được phép:<br>`sudo ufw status`
![Imgur](https://i.imgur.com/Fov9dxh.png)

Trên hệ thống, chỉ có lưu lượng SSH được cho phép thông qua, vì vậy Tôi sẽ cần thêm quy tác cho lưu lượng NFS.

Với nhiều ứng dụng, bạn có thể sử dụng `sudo ufw app list` và `enable` chúng theo tên, nhưng nfs không phải là một trong số đó. Tuy nhiên, vì `ufw` cũng kiểm tra `/etc/services ` cổng và giao thức của dich vụ, tôi vẫn có thể thêm NFS theo Tên. Thực chất tốt nhất khuyên bạn nên kích hoạt quy tắc hạn chế nhất vẫn cho phép lưu lượng truy cập bạn muốn cho phép, thay vì cho phép lưu lượng truy cập từ bất kỳ đâu.

Sử dụng câu lệnh sau để mở port `2049` trên máy **Server**, đảm bảo thay thế địa chỉ IP của máy **Client** của bạn:<br>`sudo ufw allow from client_ip to any port nfs`
![Imgur](https://i.imgur.com/EIWwNJk.png)
Bạn có thể xác minh thay đổi bằng cách: `sudo ufw status`

Bạn sẽ thấy lưu lượng được phép từ cổng `2049`:
![Imgur](https://i.imgur.com/5JnN1PM.png)
Điều này xác nhận rằng `ufw` sẽ chỉ cho phép lưu lượng NFS trên cổng `2049` từ máy Client đến.

## 5) Tạo Mount và thư mục gắn kết trên máy Client
<a name="5">
Bây giờ máy chủ **Server** được định cấu hình và phục vụ cổ phần của nó, tôi sẽ chuẩn bị máy **Client**.

Để cung cấp các chia sẻ từ xa trên máy **Client**, tôi cần gắn các thư mục trên máy **Server** mà tôi muốn chia sẻ vào các thư mục trống trên máy **Client**.
>**Lưu ý**: nếu có tệp và thư mục điểm gắn kết của bạn, chúng sẽ bị ẩn ngay khi bạn gắn kết chia sẻ NFS. Để trách mất các tập tin quan trọng, hãy chắc chắn rằng nếu bạn gắn vào một thư mục đã tồn tại thì thục mục đó là thư mục trống.

Tôi tạo hai thư mục rỗng để chuẩn bị gắn kết.
```
root@client:~# mkdir -p /nfs/general
root@client:~# mkdir -p /nfs/home
```

Bây giờ tôi có một vị trí để đặt các file từ xa và chúng đã mở tưởng lửa, có thể gắn kết nối các file bằng địa chỉ IP của máy chủ lưu trữ:
```
root@client:~# mount 192.168.10.10:/var/nfs/general /nfs/general
root@client:~# mount 192.168.10.10:/home /nfs/home
```

Các lệnh này sẽ gắn kết các Tập tin chia sẻ từ máy tính Server lên máy Client. Bạn có thể kiểm tra kỹ hơn xem chúng có gắn kết thành công hay không. Bạn có thể kiểm tra điều này một lệnh `mount` hoặc lệnh `findmnt`, hoặc `df -h` cung cấp output dễ nhìn hơn

`#sudo df -h`

![Imgur](https://i.imgur.com/rTCbvg0.png)

Cả hai file gắn kết đều đã xuất hiện bên dưới, Bởi vì chúng được gắn từ cùng một hệ thống tệp, chúng hiện thị cùng các sử dụng đĩa. Để xem có bao nhiêu không gian thực sự được sử dụng dưới mỗi điểm gắn kết, disk sử dụng lệnh `du` và đường đẫn của mount. Các flat -s cung cấp một bản tóm tắt của việc sử dụng chứ không phải là hiển thị viếc sử dụng cho tát cả các tập tin. Các -h bản in đùa ra mọi người có thể đọc được.

Ví dụ: `du -sh /nfs/home`
![Imgur](https://i.imgur.com/rW2Ek4V.png)

## 6) Kiểm tra truy cập NFS
<a name="6"></a>
Tiếp theo, hãy kiểm tra quyền truy cập vào các chia sẻ bằng cách viết một cái gì đó cho mỗi người trong số họ
### Ví dụ 1: Chia sẻ mục General 
Đầu tiên viết một tệp thử nghiệm để chia sẻ `/var/nfs/general` :

![Imgur](https://i.imgur.com/YZbkAQJ.png)

Sau đó kiểm tra quyền sở hữu:
![Imgur](https://i.imgur.com/wxaLPz3.png)

Vì tôi đã gắn khối này mà không thay đổi hành vi mặc định của NFS và tạo tệp với tư là người dùng `root` của máy Client thông qua lệnh, quyền sở hữu tệp mặc định là: các User sẽ không thể thực hiện các hành động quản trị thông thường, như thay đổi sở hữu tệp hoặc tạo thư mục mới cho một nhóm người dùng, trên tập chia sẻ gắn trên NFS này : `sudonobody:nogroup`

### Ví dụ 2: Chia sẻ thư mục `/home`
Để so sánh các quyền của chia sẻ `gerneral` với chia sẻ `/home`, hãy tạo một tệp `/nfs/home` cùng cách:

`sudo touch /nfs/home/home.txt`

Sau đó nhìn vào quyền sở hữu của tập tin:

![Imgur](https://i.imgur.com/rzoOqV3.png)

Tôi đã tạo ra `home.txt` với tư cách `root`, Chính xác như cách tôi dùng để tạo ra `NUM.txt` trong thư mục `/nfs/general`. Tuy nhiên trong trường hợp nayfm nó thuộc sở hữu của `root` vì tôi đã vượt qua hành vi mặc định khi tôi chỉ định tùy chọn `no_root_squash` trên mount này. Điều này cho phép người dùng `root` của tôi trên máy **Client** hoạt động như Root và việc giúp quản trị tài khoản người dùng thuận tiện hơn nhiều. Đồng thời,tôi không phải cấp cho những những người này quyền truy cập root trên máy **Server**

## Bước 7) Gắn thư mục Mount khi khởi động
<a name="7"></a>
Tôi có thể tự động `mount` các chia sẻ NFS khi khởi động bằng cách thêm chúng vào tệp `/etc/fstab` trên máy **Client**.

Mở tệp này với trình soạn thảo văn bản:

Ở dưới cùng them một dòng cho mỗi chia sẻ của tôi. Chúng sẽ trông như thế này:
```
...
st_ip:/var/nfs/general    /nfs/general   nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
host_ip:/home               /nfs/home      nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
```
![Imgur](https://i.imgur.com/Hz8QIUf.png)

> **Lưu ý**: Bạn có thể tìm thêm thông tin về các tùy chọn tôi đang chỉ định ở trên trong trang `man NFS`. Bạn có thể truy cập bằng cách chạy lệnh sau:<br> `man nfs`

## Bước 8) Ngắt kết nối chia sẻ Mount (Umount)
<a name="8"></a>
Nếu bện không còn muốn thư mục chia sẻ từ xa được gắn trên hệ thống của mình, bạn có thể ngắt kết nối nó bằng cách Umount ra khỏi cấu trúc thư mục của shared và ngắt kết nối như thế này, Tại máy client:
```
sudo umount /nfs/home
sudo umount /nfs/general
```

Hãy lưu ý rằng lệnh có cấu trúc là `umount` chứ không phải là `unmount` như bạn nghĩ.

Umount:
![Imgur](https://i.imgur.com/O5OJIbQ.png)

Điều này sẽ xóa các chia sẻ từ xa, chỉ để lại bộ nhớ local của bạn, kiểm tra lại
![Imgur](https://i.imgur.com/BvJWdqK.png)

Nếu bạn cũng muốn ngăn không cho chúng được nhắc lại trong lần khởi động tiếp theo, hãy chỉnh sửa file `/etc/fstab` và xóa dòng hoặc để ghi chú nó bằng cách đặt một ký tự `#` ở đầu dòng. Bạn cũng có thể ngăn việc tự động xóa tùy chọn auto, điều này sẽ cho phép bạn mount và umount thủ công.

## Kết luận
<a name="end"></a>
Trong hướng dẫn này, Tôi đã tạo ra một máy chủ NFS và minh họa một số trường hợp cụ thể NFS chính bằng cách tạo ra hai mount NFS khác nhau, tôi đã chia sẻ với máy Client NFS.

Nếu bạn đang tìm cách triển khai trong sản xuất, điều quan trọng cần lưu ý là bản thân giao thức không được mã hóa. Trong trường hợp bạn chia sẻ qua network private, điều này có thể không thành vấn đề. Trong các trường hợp khác, VPN hoặc một số loại được mã hóa khác sẽ là cần thiết để bảo vệ dữ liệu của bạn.