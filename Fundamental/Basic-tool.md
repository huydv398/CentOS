# Basic Unix tool 
Công cụ cơ bản unix

Chương này giới thiệu các lệnh để tìm hoặc định vị tệp và nén tệp cùng nhau với các công cụ phổ biến khác mà trước đây không được thảo luận
## find
Lệnh find có thể rất hữu ích khi bắt đầu một pipes để tìm kiếm tệp tin
* Tìm tất cả các tệp trong /home và đặt những gì tìm được vào trong file result.txt

`[root@sv ~]# find /home  > result.txt`
* Tìm tất cả các tệp của toàn bộ hệ thống và đưa vào danh sách allfile.txt
`[root@sv ~]# find / > allfile.txt`