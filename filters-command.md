# filter

Các lệnh được tạo để sử dụng với **pipe** thường được gọi là **filters**(bộ lọc).Những bộ lọc là những chương trình rất nhỏ làm một việc cụ thể rất hiệu quả. Chúng có thể được sử dụng để đóng gói hoặc dựng một khối.

Chương trình sẽ giúp các cho bạn các **filter** phổ biến nhất. Sự kết hợp đơn giản các lệnh và bộ lọc trong một chuỗi dài cho phép bạn thiết kế các giải pháp hữu ích.

1. ## cat
Khi ở giữa hai ống, lệnh `cat` không làm gì cả(ngoại trừ đặt **stdin** trên thiết bị xuất chuẩn).

2. ## tee
Viết các pipe trong linux,nhưng đôi khi bạn có thể muốn có kết quả trung gian. Điều này là `tee`, và `tee` gần giống với `cat` ngoại trừ nó có hai đầu ra giống hệt nhau.

3. ## grep
`grep` là bộ lọc phổ biến với người dùng Unix. Việc sử dụng grep phổ biến nhất là lọc các dòng của văn bản chứa( hoặc không chứa) một chuỗi nhất định.
```
[huyts@localhost ~]$ cat count.txt
112
1134
231
541
122
567
443
111
4444
[huyts@localhost ~]$ cat count.txt | grep 1
112
1134
231
541
122
111
[huyts@localhost ~]$ cat count.txt | grep 122
122
```

Bạn cũng có thể viết điều này mà không cần `cat`
```
[huyts@localhost ~]$ grep 122 count.txt
122
```
Một trong những tùy chọn hữu ích nhất của grep là `grep -i` lọc theo cách không phân biệt chữ hoa chữ thường.
```
[huyts@localhost ~]$ grep huy name.txt
[huyts@localhost ~]$ grep -i  huy name.txt
DUONG VAN HUY
Duong Van Huy
```
`grep -v` đưa ra các dòng không khớp với chuỗi.
```
[huyts@localhost ~]$ grep 8 count.txt
[huyts@localhost ~]$ grep -v 8 count.txt
112
1134
231
541
```
Và hai tùy chọn trên có thể kết hợp với nhau (`grep -vi`) để lọc tất cả các dòng không chứa chuỗi không phân biệt chữ hoa chữ thường.

Với `grep -A1`(after) Hiển thị 1 dòng tiếp theo của kết quả, `grep -B1`(before) hiển thị 1 dòng trước kết quả và ` grep -C1` (context) hiển thị 1 dòng trước và một dòng sau của kết quả.Tất cả ba tùy chọn(A,B,C) có thể hiện thị bất kỳ số lượng dòng nào(ví dụ:A2, B4, C10)
4. ## cut
Bộ lọc cắt có thể chọn các cột từ các tệp, tùy thuộc vào số phân cách và số byte.
Hiển thị bên dưới sử dụng cut để lọc tên người dùng và user trong tệp **/etc/passwd** Nó sử dụng dấu hai chấm(:) làm dấu phân cách và chọn các trường 1 và 3
```
[huyts@localhost ~]$ cut -d: -f1,3 /etc/passwd | tail -4
huy1:1003
huy2:1004
huy3:1005
huy4:1006
```
Ví dụ này sử dụng **cut** để hiện thị ký tự thứ 2 đến ký tự thws7 của tệp **/etc/passwd**
```
[huyts@localhost ~]$ cut -c2-7 /etc/passwd | tail -4
uy1:x:
uy2:x:
uy3:x:
uy4:x:
```
5. ## tr
Vơi `tr` có thể thay thế các ký tự trong tệp.
```
[huyts@localhost ~]$ cat name.txt
DUONG VAN HUY
Duong Van Huy
Nguyen ngoc mInh
[huyts@localhost ~]$ cat name.txt |tr '[a-z]' '[A-Z]'
DUONG VAN HUY
DUONG VAN HUY
NGUYEN NGOC MINH
```
`tr -s` cũng có thể được sử dụng để ép nhiều lần xuất hiện của một ký tự thành một.
```
[huyts@localhost ~]$ cat num.txt
mot    hai  ba  bon

nam     sau  bay
[huyts@localhost ~]$ cat num.txt | tr -s ' '
mot hai ba bon

nam sau bay
```
`tr -d` dùng để xóa các ký tự
6. ## wc
`wc` sử dụng để đếm từ, dòng và ký tự dễ dàng.
```
[huyts@localhost ~]$ wc num.txt
 3  7 38 num.txt
```
