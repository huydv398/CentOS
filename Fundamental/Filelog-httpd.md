# Log file

## Tổng quan

**Httpd Apache** cung cấp các cơ chế khác nhau, để ghi lại mọi thứ xảy ra trên máy chủ vào file log

* Yêu cầu ban đầu
* Quy trình đường dẫn URL 
* Mọi lỗi có thể xảy ra trong quy trình

Các module có thể cung cấp khả năng ghi nhật ký hoặc đưa các mục vào tệp nhật ký hiện có.

## Error_log
Là nơi Apache Http sẽ gửi thông tin chuẩn đoán và ghi lại bất kỳ lỗi nào mà nó gặp phải trong các yêu cầu xử lý.

*ErrorLogFormat* : `[Mon Jun 08 09:44:36.431276 2020] [lbmethod_heartbeat:notice] [pid 1797] AH02282: No slotmem from mod_heartmonitor`

* `[Mon Jun 08 09:44:36.431276 2020]`: 
* `[lbmethod_heartbeat:notice]`: Module tạo ra thông điệp
* `[pid 1797]`: Process ID 
* `AH02282: No slotmem from mod_heartmonitor`:  Thông báo lỗi.

## Access log 
Là nơi ghi lại tất cả các yêu cầu được xử lý bởi máy chủ. 

```
192.168.20.1 - - [08/Jun/2020:14:38:44 +0700] "POST /wp-admin/admin-ajax.php HTTP/1.1" 200 47 "http://192.168.20.2/wp-admin/tools.php" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36"
```

Bao gồm:
* `192.168.20.1`: Địa chỉ IP của máy khách đã gửi yêu cầu đến máy chủ/
* User Name của Client
* `[08/Jun/2020:14:38:44 +0700]`: Thời gian múi giờ
* Phương thức GET hoặc POST thuộc Giao thức Http
    * Yêu cầu tài nguyên từ đâu :`/wp-admin/admin-ajax.php`
    * Sử dụng giao thức Http nào: `HTTP/1.1`
* `200`: Mã trạng thái máy chủ gửi cho khách
* `47`: Cho biết kích thước của đối tượng được trả về máy khách

* `http://192.168.20.2/wp-admin/tools.php`: Tiêu đề giới thiệu. Cung cấp trang trước của khách khi vào đây .

* `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36`: Tác nhân của người dùng, thông tin nhận dạng mà máy client báo cáo về nó

## Thống kê số lần truy cập của các IP vào web

Hiển thị tất cả các IP có trong file Accesslog

**Cách 1** thống kê bằng câu lệnh Grep
* Với câu lệnh, hiển thị ngẫu nhiên không theo sắp xếp thứ tự:

`grep -o "[0-9]\+\.[0-9]\+\.[0-255]\+\.[0-9]" /var/log/httpd/access_log | sort | uniq`

![Imgur](https://i.imgur.com/eYHiY3A.png)

* Với câu lệnh, Hiện thị tổng số lần xuất hiện theo thứ tự tăng dần:

`grep -o "[0-9]\+\.[0-9]\+\.[0-255]\+\.[0-9]" /var/log/httpd/access_log | sort | uniq -c | sort -n`

![Imgur](https://i.imgur.com/Y8BVzxV.png)

* Với câu lệnh, Hiện thị tổng số lần xuất hiện theo thứ tự giảm dần :

`grep -o "[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /var/log/httpd/access_log | sort | uniq -c | sort -nr`

![Imgur](https://i.imgur.com/tXsstGn.png)



**Cách 2** Hiển thị những địa chỉ IP có ở phần đầu của file access log

`awk '{print $1}' /var/log/httpd/access_log | sort | uniq -c | sort -nr`

![Imgur](https://i.imgur.com/rUiMFIr.png)