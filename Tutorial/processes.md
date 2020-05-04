# Linux Process - Các quy trình trong Linux
Một **Processes** (tiến trình) chỉ đơn giản là thể hiện một hoặc nhiều tác vụ liên quan thực thi trên cùng một máy. Nó không giống như một chương trình hoặc một lệnh; Một chương trình duy nhất thực sự có thể bắt đầu một quy trình cùng một lúc. Một số quy trình độc lập với nhau và những quy trình khác có liên quan. Lỗi của một quá trình có thể hoặc hoặc không thể ảnh hưởng đến quá trình khác đang chạy trên hệ thống. Các quy trình sử dụng nhiều tài nguyên hệ thống, như bộ nhớ, chu kỳ CPU và các thiết bị ngoại vi như máy in và màn hình. Hệ điều hành ( đặc biệt là kernel ) chịu trắc nhiệm phân bổ một phần thích hợp các tài nguyên này cho từng quy trình và đảm bảo sử dụng tối ưu tổng thể.

Tại bất cứ thời điểm nào, luôn có nhiều quá trình được thực thi. Hệ điều hành theo dõi chúng bằng cách cho mỗi quy trình là 1 **PID -Process ID** duy  nhất. **PID** được sử dụng để theo dõi trạng thái quá trình, chính xác là nơi tài nguyên được đặt trong bộ nhớ và các điểm đặt khác. Các bộ vi sử lý mới thường được chỉ định theo thứ tự tăng dần khi các quy trình được sinh ra.Do đó **PID** biểu thị một quá trình khởi tạo và các quá trình thành công dần dần được gắn con số lớn hơn.

Linux cho phép bạn thiết lập và các thao tác ưu tiên quy trình. Các quy trình ưu tiên cao hơn được cấp nhiều thời gian hơn trên
## Tìm hiểu định nghĩa và thông tin:CPU, Socket, Program, Processes, Thread
### CPU 
* CPU (Central Processing Unit) là bộ xử lý trung tâm, là các mạch điện tử trong maysm nhiệm vụ của CPU là xử lý thông tinm tính toán các dữ liệu, nhận biết các thao tác của người dùng để điều khiển các hoạt động máy tính,

Hiện có 2 hãng công nghệ lớn chuyên sản xuất CPU cho máy tính để bàn và laptop lần lượt là Inter và AMD.
* CPU gồm những gì?
  * CPU được cấu thành từ hàng triệu transistor ( bóng bán dẫn) được sắp xếp với nhau tạo thành mạch điện tử.
  * CPU chia làm 3 phần: Control Unit( Khối điều khiển-CU) và Arithmethic Logic Unit(Khối tính toán -ALU )
* Các thông số có trên CPU
 * Cỏe( Nhân): các nhân của CPU sẽ đảm nhiệm những quá trình xử lý khác nhau, thông thường những CPU có càng nhiều nhân càng tốt vì ddieuf đó sẽ giúp CPU xử lý các chương trình đa tác vụ. Tuy nhiên, chúng ta cũng nên quan tâm đến xung nhịp của mỗi nhân(GHZ) vì nó thể hiện tốc độ xử lý từng nhân riêng lẻ trên CPU, tất nhiên là xung càng cao CPU càng mạnh. Một điểm lưu ý rằng: tùy vào chương trình chúng ta sử dụng trên máy tính cho phép dùng số lượng nhân nhất định. Với những chương trình chạy ít nhân thì các CPU ít nhân nhưng xung nhịp cao sẽ được ưu tiên hơn CPU nhiều nhân nhưng xung nhịp thấp và ngược lại
 * Thread( luồng): Nếu ta tưởng tượng các core là của CPU dùng để chia nhỏ công việc của một trương trình thì thread là dạng chia nhỏ công việc lần thứ 2.
 * Cache( Bộ nhớ đệm): dùng để lưu trữ các lệnh chuẩn bị xử lú của CPU, Ở đây các lệnh sẽ đucợ xếp thành 1 hàng và tất nhiên bộ nhớ đệm càng cao thì hiệu xuất CPU càng cao.
* Socket
 * Socket CPU là đế của các CPU trên mainboard, Tương thích với từng loại Mainboard là các đế cắm khác nhau, từ đó sẽ phù hợp với các loại CPU khác nhau
 * Socket có nhiệm vụ làm điểm tiếp xúc và cũng là giá đỡ CPU khi gắn vào Mainboard
 * Sử dụng socket giảm thiểu được rất nhiều rủi ro trong việc làm vỡ hoặc cong các chân của CPU khi lắp đặt hoặc gỡ bỏ nó.
* Process
 * Tiến trình (Process) chỉ là sự thể hiện của một hoặc nhiều tác vụ liên quan đến thực thi trên máy tính của bạn. Nó không giống như một chương trình hoặc một cửa sổ dòng lệnh. Một lệnh duy nhất thực sự bắt đầu một tiến trình cùng một lúc. Một số tiến trình độc lập với nhau và những tiến trình có liên quan. Lỗi của một tiến trình có thể ảnh hưởng đến các tiến trình khác đang chạy trên hệ thống
 * Cá tiến trình sử dụng nhiều tài nguyên hệ thống: bộ nhớ, CPU và các thiết bị ngoại vi
 * Hệ điều hành chịu trách nhiệm phân bổ phần tích hợp các tài nguyên nay cho từng quy trình và đảm bảo việc sử dụng hệ thống được tối ưu hóa tổng thể.
* Thread 
* Thread là một đơn vị cơ bản trong CPU. Một Luồng sẽ chia sẻ với các luồng khác trong cùng process về thông tin dât, các dữ liệu của mình. Việc tạo ta Thread giúp cho các chương trình có thể chạy được nhiều công việc cùng một lúc

### Các trạng thái của một tiến trình Process
Các bước chuyển quá trình của một process:
* Những Process được chương trình tạo ra, được sắp vào "hàng đợi"(hay còn được gọi là Stack)
* Process được CPU thấy và thực thi
* Khi Process quá tải, CPU phải nhả để thực hiện Process khác.
* Khi Process được thực hiện xong.
* Khi Process đang thực hiện và yêu cầu I/O hay các tín hiệu khác.(ví dụ bạn cần file word in ra Process phải nói cho CPU, CPU gửi tín hiệu cho Máy in, Máy in in ra rồi báo lại CPU.Trong thời gian đó, để tiết kiệm thời gian Process này sẽ được chuyển qua trạng thái chờ, cho Process khác vào thực hiện)

### Các loại tiến trình 
|Loại tiến trình|Mô tả|
|-|-|
|Tiến trình tương tác|Được bắt đầu bởi người dùng, tại một dòng lệnh hoặc thông qua giao diện đồ họa như tượng hay lựa chọn trong menu|
|Tiến trình Batch|Các tiến trình tự đọng được lên lịch bắt đầu kết nối và sau đó ngắt kết nối khỏi terminal. Các tác vụ này được sắp xếp và hoạt động trên cơ sở|
|Deamons|Máy chủ xử lý chạy liên tục. Nhiều người được khởi chạy trong quá trình khởi động hệ thống. Sau đó, chờ người dùng hoặc yêu cầu hệ thống cho biết rằng dịch bụ nào là bắt buộc.Ví dụ:http,sshd,...|
|Threads|Các tiến trình nhẹ là các tác vụ chạy dưới trình chính nhằm chia sẻ bộ nhớ và các tài nhuyên khác, được hệ thống lên lịch và chỵ trên cơ sở cá nhân. Một luồng có thể kết thúc mà không kết thúc toàn bộ tiến trình và môt tiến trình có thể tạo các luồng mới bất cứ lúc nào.Ví dụ,firefox,...|
|Kernel Threads|Các tác vụ nhân mà người dùng không bắt đầu cũng không kết thúc và có ít quyền kiểm soát.Chúng có thể thực hiện các hành động như di chuyển một luồng từ CPU này xang CPU khác, Hoặc đảm bảo các hoạt động đầu vào/ đâu ra vào được đĩa hoàn thành|
### ID User và ID Group
Nhiều người dùng có thẻ truy cập vào hệ thống cùng một lúc và mỗi người dùng đều chạy nhiều tiến trình khác nhau. Hệ điều hành Linux sẽ xác định người dùng bắt đầu tiến trình bằng ID người dùng.
### Ưu tiên trong Linux
Tại một thời điểm, Nhiều tiến trình đang chạy trong một hệ thống. Tuy nhiên, CPU thực sự chỉ có thể chứa một nhiệm vụ tại một thời điểm.Các tiến trình ưu tiên cao hơn được cấp nhiều thời gian hơn trên CPU. Mức độ ưu tiên cho một tiến trình có thể đặt bằng cách chỉ định một giá trị cho tiến trình. Giá trị càng thấp, mức độ ưu tiên càng cao
||Process 1|Process 2|Process 3|Process n|
|-|-|-|-|-|
|Nice Value|-20|-19|19|
|Elapsed Time|0|1|2|n|

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
