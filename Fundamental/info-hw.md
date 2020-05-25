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
