# Linux Process - Các quy trình trong Linux
Một **Processes** (tiến trình) chỉ đơn giản là thể hiện một hoặc nhiều tác vụ liên quan thực thi trên cùng một máy. Nó không giống như một chương trình hoặc một lệnh; Một chương trình duy nhất thực sự có thể bắt đầu một quy trình cùng một lúc. Một số quy trình độc lập với nhau và những quy trình khác có liên quan. Lỗi của một quá trình có thể hoặc hoặc không thể ảnh hưởng đến quá trình khác đang chạy trên hệ thống. Các quy trình sử dụng nhiều tài nguyên hệ thống, như bộ nhớ, chu kỳ CPU và các thiết bị ngoại vi như máy in và màn hình. Hệ điều hành ( đặc biệt là kernel ) chịu trắc nhiệm phân bổ một phần thích hợp các tài nguyên này cho từng quy trình và đảm bảo sử dụng tối ưu tổng thể.

Tại bất cứ thời điểm nào, luôn có nhiều quá trình được thực thi. Hệ điều hành theo dõi chúng bằng cách cho mỗi quy trình là 1 **PID -Process ID** duy  nhất. **PID** được sử dụng để theo dõi trạng thái quá trình, chính xác là nơi tài nguyên được đặt trong bộ nhớ và các điểm đặt khác. Các bộ vi sử lý mới thường được chỉ định theo thứ tự tăng dần khi các quy trình được sinh ra.Do đó **PID** biểu thị một quá trình khởi tạo và các quá trình thành công dần dần được gắn con số lớn hơn.

Linux cho phép bạn thiết lập và các thao tác ưu tiên quy trình. Các quy trình ưu tiên cao hơn được cấp nhiều thời gian hơn trên

## Running Processes
### Các lệnh `ps` 
cung cấp thông tin về tiến trình đang chạy,
* command: `ps [option]`
  * option:
    * `-u` : Để hiện thị thông tin của các quy trình cho tên người dùng chỉ định
    * `-ef` : Để hiện thị tất cả các quy trình trong hệ thống một cách chi tiết
    * `-eLf`: Tiến thêm một bước và hiển thị thông tin cho mỗi luồng( một quá trình có thể chứa nhiều luồng)

```
[huyts@localhost ~]$ ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 09:31 ?        00:00:02 /usr/lib/systemd/systemd --switched-
root          2      0  0 09:31 ?        00:00:00 [kthreadd]
root          4      2  0 09:31 ?        00:00:00 [kworker/0:0H]
```
Ý nghĩa output lệnh `ps`:
|Name|Ý nghĩa|
|-|-|
|UDI|UserID mà Process này thuộc về( người chạy nó)|
|PID|Process ID|
|PPID|Process ID gốc(ID của Process mà nó bắt đầu)|
|C|CPU sử dụng của Process|
|STIME|Thời gian bắt đầu Process|
|TTY|Kiểu teminal liên kết với Process|
|TIME|Thời gian CPU bị sử dụng bởi Process|
|CMD|Lệnh bắt đầu Process|
### Các lệnh `top`
Ý nghĩa tương tự `ps`, nhưng `top` có thể được cập nhật theo thời gian liên tục( cứ sau 2 giây theo măc định)
- phím q - để thoát khỏi `top`
```
[huyts@localhost ~]$ top
top - 12:48:19 up  3:16,  2 users,  load average: 0.00, 0.01, 0.05
Tasks: 101 total,   1 running, 100 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.3 us,  0.0 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :   995748 total,   699844 free,   174416 used,   121488 buff/cache
KiB Swap:  2097148 total,  2097148 free,        0 used.   685272 avail Mem

   PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
     1 root      20   0  128012   6596   4160 S  0.0  0.7   0:02.03 systemd
     2 root      20   0       0      0      0 S  0.0  0.0   0:00.00 kthreadd
     4 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kworker/0:0H
     5 root      20   0       0      0      0 S  0.0  0.0   0:00.18 kworker/u256:0
     6 root      20   0       0      0      0 S  0.0  0.0   0:00.12 ksoftirqd/0
```
Dòng đầu tiên của top hiển thị một bản tóm tắt nhanh về những gì đăng xảy ra trong hệ thống  bao gồm:
1. Hệ thống đã hoạt động trong bao lâu
2. Có bao nhiêu người dùng đang đăng nhập
3. Thông tin về load average

Dòng 2 hiển thị tổng số Process, số lượng quá trình chạy,sleep,stop và zombie.

Dòng 3 hiển thị Thông tin CPU, sự phân chia giữa
* us - người dùng
* sy -kernel
* ni - mức ưu tiên thấp
* id - chế độ không tải
* wa - công việc đang chờ
* hi - ngăt phần cưng
* si - ngắt phần mềm 
* st - steal time: thường sử dụng với các máy ảo

Dòng 4 và 5 biểu thị múc độ sự dụng bộ nhớ, được chia làm 2 loại:
* Dòng 4 - hiển thị bộ nhớ ram vật lý
* Dòng 5 - Swap space
* cả hai đều hiển thị tổng bộ nhớ, bộ nhớ đã sử dụng và không gian trống.

Các thông số hiện thị trong `top`
|Name|Ý nghĩa|
|-|-|
|PID|Số Process|
|USER|Chủ sở hữu Process|
|PR|Ưu tiên|
|NI|Giá trị nice|
|VIRT|Virtual |
|RES|Physical |
|SHR|Bộ nhớ chia sẻ|
|S|status trạng thái|
|%CPU|Tỷ lệ % của CPU|
|TIME +|Thời gian thực hiện|
|COMMAND|Lệnh|
|$ MEM|Bộ nhớ được sử dụng|

### kill
Là lệnh tắt Process đang chạy. Khi sử dụng lệnh `kill` với một tiến trình con thì chỉ tiến trình đó  được tắt nhưng nết sử dung `kill` với tiến trình của cha thì toàn bộ con của nó cũng được tắt theo
* Command : `kill [option] [pid]`
    * `-9` : kill toàn bộ các process liên quan

## so sánh `ps` và `top`

* `ps` chỉ hiện thị thông tin từ dòng thứ của lệnh `top`
* Nếu `top` hiện thị một danh sách realtime các tiến trình thì `ps` chỉ hiện thị thông tin tại thời điểm khởi chạy lệnh.
* `top` và `ps` đều có thể dùng kết hợp với pipe tuy nhiên như vậy thì tính tealtime của `top` sẽ không có ý nghĩa.
