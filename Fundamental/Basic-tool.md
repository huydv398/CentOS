# Basic Unix tool 
Công cụ cơ bản unix

Chương này giới thiệu các lệnh để tìm hoặc định vị tệp và nén tệp cùng nhau với các công cụ phổ biến khác mà trước đây không được thảo luận
## find
Lệnh find có thể rất hữu ích khi bắt đầu một pipes để tìm kiếm tệp tin
* Tìm tất cả các tệp trong /home và đặt những gì tìm được vào trong file result.txt
  * `[root@sv ~]# find /home  > result.txt`
* Tìm tất cả các tệp của toàn bộ hệ thống và đưa vào danh sách allfile.txt
  * `[root@sv ~]# find / > allfile.txt`
* Tìm tất cả các tệp kết thúc bằng .txt trong thư mục hiện tại
  * `[root@sv ~]# find . -name "*.txt"`
* Tìm các tệp thuộc loại tệp(không phải thư mục,pipe hoặc etc) cos đuôi kết thúc là .conf 
  * `[root@sv ~]# find . -type f -name "*.conf"`
* Tìm các tệp mới hơn tên result.txt 
  * `[root@sv ~]# find . -newer result.txt`
## locate
Locate là một công cụ định vị rất khác so với find ở chỗ nó sử dụng một chỉ mục để định vị các tệp.
## date
Lệnh `date` có thể hiển thị ngày, giờ, múi giờ và hơn thế nữa.
```
[huy@sv ~]$ date
```
Thời gian bất kỳ trên Unix nao fđược tính bằng số giây kể từ năm 1969( giây đâu tiên được tính là thứ hai đầu tiên của tháng một năm 1970)

Khi nào giây sẽ đạt hai nghìn triệu?

`[huy@sv ~]$ date -d '1970-01-01 + 2000000000 seconds'
`
## cal 
Lệnh hiện thị tháng hiện tại và ngày hiện tại được tô sáng.
```
[huy@sv ~]$ cal
      May 2020
Su Mo Tu We Th Fr Sa
                1  2
 3  4  5  6  7  8  9
10 11 12 13 14 15 16
17 18 19 20 21 22 23
24 25 26 27 28 29 30
31
```
## sleep
Lệnh ngủ đôi khi được sử dụng trong các tệp lệnh để chờ một vài giây. Sau khi hoàn thành máy trở lại bình thường
## time
Lệnh `time` có thể hiển thị thời gian cần thiết để thực hiện lệnh.
## gzip gunzip
Người dùng không bao giờ có đủ dung lượng đĩa, vì vậy việc nén có ích.Lệnh gzip có thể làm cho các tệp tin chiếm ít không gian hơn
```
[huy@sv ~]$ ls -lh
-rw-rw-r--. 1 huy huy 4.5M May  4 11:48 file.txt
[huy@sv ~]$ gzip file.txt
[huy@sv ~]$ ls -lh
-rw-rw-r--. 1 huy huy 435K May  4 11:48 file.txt.gz
[huy@sv ~]$
```
Bạn cũng có thể lấy lại bản gốc với unzip
```
[huy@sv ~]$ gunzip file.txt.gz
[huy@sv ~]$ ls -lh
total 4.5M
-rw-rw-r--. 1 huy huy 4.5M May  4 11:48 file.txt
```
## zcat -zmore
Các tệp văn bản được nén bằng gzip có thể được xem bằng `zcat` và `zmore`
```
[huy@sv ~]$ gzip num.txt
[huy@sv ~]$ ls
file.txt  num.txt.gz
[huy@sv ~]$ zcat num.txt.gz | head -4
1
2
3
4
```
## bzip2 và bunzip2
Các tệp tin cũng có thể nén bằng bzip2, mất nhiều thời gian hơn gzip,nhưng nén tốt hơn.(cách làm tương tự như gzip)
## bzcat và bzmore
Và theo cách tương tự , có thể hiện thị tệp được nén bằng bzip2