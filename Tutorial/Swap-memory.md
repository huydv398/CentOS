# Bộ nhớ trao đổi Linux
Linux chia RAM thành các vùng nhớ gọi là trang. Các trao đổi là quá trình mà một trang bộ nhớ được sao chép vào một không gian đĩa cứng được cấu hình sẵn, được gọi là không gian trao đổi, để phát hành từ bộ nhớ. Kích thước kết hợp của bộ nhớ vật lý và không gian trao đổi là năng lượng bộ nhớ ảo có sẵn. Trao đổi là cần thiết vì hai lý do quan trọng:
* Khi hệ thống thống yêu cầu nhiều bộ nhớ hơn khả năng vật lý, kernel sẽ di chuyển các process ít sử dụng vào không gian SWAP và cấp quyền cho bộ nhớ ram cho ứng dụng hiện tại hiện đang yêu cầu bộ nhớ.
* Một số lượng đáng kể các trang được sử dụng bởi một ứng dụng trong giai đoạn khởi động của nó chỉ có thể được sử dụng để khởi tạo hệ thống và không bao giờ được sử dụng lại.

Do đó, hệ thống có thể sử dụng trao đổi trên các trang đó và giải phóng bộ nhớ cho các ứng dụng khác hoặc thậm chí cho bộ đệm trên đĩa. Tuy nhiên, trao đổi có một nhược điểm. So với RAM, Đĩa chậm hơn nhiều. Tốc độ bộ nhớ được đo bằng nano giây, trong khi tốc độ đĩa được đo bằng mili giây, do đó truy cập vào đĩa chậm hơn chục nghìn lần so với bộ nhớ ram. Càng nhiều hoán đổi xảy ra, Hệ thống của bạn sẽ càng chậm. Đôi khi hoán đổi mức cao tạo ra các nút cổ chai, Như một tình huống cụ thể xảy ra: Một trang bị tráo đổi xảy ra và sau đó lan man rất nhanh và liên tục. Trong những tình huống như vậy, hệ thống phải vật lộn để tìm bộ nhớ trống và giữ cho các ứng dụng khác nhau chạy cùng một lúc. Trong trường hợp này, chỉ thêm nhiều ram sẽ giúp ổn định hệ thống.

Linux có hai dạng không gian hoán đổi: Phân vùng tạo hoán đổi và tệp hoán đổi. Phân vùng trao đổi là là một phần độc lập của đĩa cứng, được sử dụng riêng cho trao đổi, không ai khác có thể cư trú ở đó. Tệp hoán đổi là tệp đặc biệt nằm trong hệ thống tệp giữa hệ thống và tệp dữ liệu. Để xem nó được tạo ra như thế nào và vị trí không gian hoán đổi mà bạn sở hữu.

## Swap Memory là gì?
Swap memory được sử dụng khi hệ thống của bạn quyết định rằng nó cần thêm bộ nhớ ram cho quá trình hoạt động và bộ nhớ ram hiện tại không còn đủ để sử dụng. Nếu điều đó xảy ra, các tài nguyên và dữ liệu tạm thời không hoạt động trên bộ nhớ ram sẽ được di chuyển để lưu trữ vào không gian Swap để giải phóng bộ nhớ ram để sử dụng vào việc khác

Lưu ý rằng thời gian truy cập vào vùng swap là chập hơn nhiều, do đó bạn không nên coi việc sử dụng Swap là một phương pháp thay thế cho ram.
* Là khái niệm của bộ nhớ ảo được sử dụng trên Linux. Nếu hết RAM hệ thống sẽ tự động dùng một phần ổ cứng để làm bộ nhớ cho các ứng dụng hoạt động
## Khi nào cần phải sử dụng bố nhớ Swap Memory
* **Tối ưu hóa bộ nhớ**: Hệ thống sẽ di chuyển các tài nguyên và dữ liệu hiện không được sử dụng trong bộ nhớ ram đến Swap, điều này giúp hệ thống phục vụ các mục đích khác tốt hơn.
* **Tránh các trường hợp không lường trước**: Trong một số trường hợp, Bạn không dự tính được bộ nhớ dành cho các chương trình mà bạn chuẩn bị thử nghiệm, hoặc một  trương trình bất kỳ nào đó nổi điên lên, hoặc bất cứ điều gì đó bất thường
Các bước tạo Swap File 
* Kiểm tra phân vùng Swap : `swapon -s`
* Kiểm tra dung lượng còn trống trên ổ cứng: `df -TH`
* Tạo file Swap, tùy biến dung lượng 1G :`fallocate -l 1G /swapfile`

* Nếu hệ thống không có sẵn `fallocate`. Tạo Swap File:
```
dd if=/dev/zero of=/swapfile bs=1024 count=1048576

bs: kích thướng Swap File
count: tốc độ
```
* Phân quyền cho file vừa tạo. Set mod = 600 cho chỉ có root user mới có quyền truy cập:
    * ` chmod 600 /swapfile `
    * `chown root:root swapfile`

* Sử dụng `mkswap` để thiết lập file trở thành file Swap:
    * `mkswap /swapfile`
* Khởi động Swap File bằng lệnh sau: 
    * ` swapon /swapfile `
* Mở file `/etc/fstab` và thêm vào cuối dòng sau :`/swapfile swap swap defaults 0 0`
## Kiểm tra lại vùng Swap 
* `swapon --show`
## Giá trị Swappiness 
Giá trị Swappiness từ 0 - 100, giá trị mặc định ở `30`, chỉ số này càng thấp thì máy linux sẽ càng tránh sử dụng Swap,giá trị càng cao thì linux cang ưu tiên sử dụng, chúng ta có thể thau đổi giá trị này bằng câu lệnh:
* Command: `sysctl vm.swappiness=10`
    * `10` : là giá trị có thể thay đổi khi bạn có muốn ưu tiên sử dụng swap không

* **Swappiness** có giá trị từ 0 -> 100:
    * `0`: **swap** được sử dụng khi ram bị sử dụng hết
    * `10`: **swap** được sử dụng khi ram còn `10%`
    * `60`: **swap** được sử dụng khi ram còn `60%`
    * `100`: **swap** được sử dụng ưu tiên như là RAM

=> Do **Swap** chậm hơn Ram nên đặt **swappiness** về gần 0 hoặc chỉnh là 10 
* Kiểm tra mức độ dùng của hệ thống bằng lệnh:
    * `# cat /proc/sys/vm/swappiness`
* Lưu thông số **swappiness** vào file `/etc/sysctl.conf` thêm dòng `vm.swappiness = 10` và khởi động lại server.

## Thay đổi dung lượng Swapfile 
* tắt swapfile:
    * `swapoff /swapfile `
* Xóa file Swap :
    * `rm -f /swapfile`
* Tạo file swap với dung lượng mong muốn:
    * `# dd if=/dev/zero of=/swapfile bs=1M count=4096`
* Tạo phân vùng swap mới:
    * ` mkswap /swapfile`
* Kích hoạt Swap :
    * `swapon /swapfile`
* Bảo mật file swap:
    * `chown root:root /swapfile`
    * `chmod 0600 /swapfile`
* Kiểm tra lại swap :`swapon -s`
>***lưu ý** Khi thay đổi dung lượng, **swapiness** vẫn được giữ nguyên.*
