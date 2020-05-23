# Giao thức NTP(Network Time Protocol) là gì?
* Giao thức NTP là giao thức được sử dụng để đồng bộ hóa thời gian của đồng hồ máy tính.
## Các thành phần hoạt động trong NTP 
* NTP Server và NTP Client.
* NTP Client trao đổi yêu cầu thời gian với máy NTP Server. Nếu máy Client lệch giờ với máy Server, CLient sẽ tự điều chỉnh
## Các mô hình NTP.
* Có 2 mô hình
    * Mô hình 1, Server đồng bộ hóa thời gian với server quốc tế.<br>Thích hợp với mô hình máy lẻ.
    * Mô hình 2:  
        * Node 1 có một máy Server lấy thời gian từ server quốc tế. 
        * Node 2 là các máy bên dưới đồng bộ hóa thời gian từ máy server.<br>=> Mô hình thích hợp cho mạng lưới có nhiều máy Client liên tuc đồng bộ hóa thời gian tiết kiệm băng thông quốc tế.

### Với mô hình 1
* Cần chuẩn bị mô hình :
    * Một máy có cài đặt hệ điều hành.
    * Đươc truy cập dưới quyền `root`.
    * Có kết nối Internet.
* Chuẩn bị máy trước khi cài đặt
    * Cấu hình Timezone.
    * Cấu hình Allow Firewall.
    * Disable SElinux
    
* Các bước cài đặt:
    * Cài đặt Chrony.
    * Khởi động dịch vụ
    * Sửa file cấu hình `/etc/chrony.conf`
        * Sửa địa chỉ máy sẽ pool để đồng bộ thời gian
    * Restart và kiểm tra lại dịch vụ

### Với mô hình 2
* Cần chuẩn bị mô hình:
    * Hai máy đã cài đặt hệ điều hành.
    * Hai được quyền truy cập root
    * Có hai máy có truy cập Internet(Khi cài đặt xong Chrony máy client có thể donw Internet)
* Chuẩn bị máy trước khi cài đặt
    * Cấu hình Timezone.
    * Cấu hình Allow Firewall.
    * Disable SElinux.
* Cài đặt Chrony trên cả 2 server 
    * Cấu hình Server làm NTP Server:
        * Sửa cấu hình cho phép các máy Client đồng bộ hóa thời gian.(`#allow [ip/sm]`)
        * Nếu máy Server đồng bộ hóa từ giờ quốc tế.Thêm cấu hình pool.
        * restart và kiểm tra.
    * Cấu hình Server làm NTP Client:
        * Cấu hình để máy Client đồng bộ hóa từ máy Server.
        * Restart lại dịch vụ và kiểm tra.
