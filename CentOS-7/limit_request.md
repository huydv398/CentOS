# Giới hạn số lượng request trong một trong một khoảng thời gian.

Việc giới hạn số lượng request của một client trong một khoảng thời gian sẽ giảm được rủi to server bị tấn công DDoS. Bạn tính toán với trang web của mình một người dùng bình thường sẽ không thực hiện quá 1 request trong vòng 2 giây, còn nếu quá thì chắc chắn người dùng này đang có hành động bất thường. Như vậy ta sẽ giới hạn số lượng request cho một client trong 1 phút chỉ có thể thực hiện tối đa 30 request.

## Tiến hành cấu hình 

* Chuẩn bị server đã cài sẵn LEMP.

### Cấu hình tại file nginx.conf
Mở file `nginx.conf`

`vi /etc/nginx/nginx.conf`

Thêm dòng sau vào block `http{}`

`limit_req_zone  $binary_remote_addr  zone=one:10m   rate=40r/m;`

Sau khi thêm vào tệp `nginx.conf`

![Imgur](https://i.imgur.com/ZhtTHwj.png)


* `zone=one:10mb` Tạo ra một vùng nhớ làm mục đính gắn với các trong web khác nhau với các bảo mật khắc nhau. Ở đây sẽ tạo ra một vùng nhớ có tên `one` có dung lượng `10MB` để lưu trữ trạng thái của của request theo kiểu key-value(trong trường hợp này là địa chỉ client). 

* `rate=40r/m` Để chỉ ra số request giới hạn là 40 request trong vòng 1 phút. 

### Cấu hình tại block
Bây giờ bạn muốn giới hạn request có hiệu lực ở đâu thì bạn đặt `limit_reg zone=one;` trong các block đó. 

* Nếu bạn muốn nó áp dụng cho tất cả các trang web trên nginx này thì bạn đặt trong block `http{}`.

!

* Nếu muốn áp dụng cho một trang web riêng biệt thì đặt vào trong block `server {}`.

![](https://i.imgur.com/BYBnzdL.png)

* Muốn giới hạn trên một số trường hơp xác định chính xác thì đặt nó trong block `location {}`

![](https://i.imgur.com/ywFnxFZ.png)

Như trường hợp này thì mọi request vào đường dẫn `[domain]/banner/` thì sẽ giới hạn lượng truy cập

![](https://i.imgur.com/UtUVywA.png)


> **Chú ý**: Sau khi chỉnh sửa file cấu hình. Kiểm tra bằng câu lệnh: <br> `nginx -t`<br> Nếu hiển thị như bên dưới là câu lệnh không bị lỗi:<br>`nginx: the configuration file /etc/nginx/nginx.conf syntax is ok`<br>
`nginx: configuration file /etc/nginx/nginx.conf test is successful` <br> 

Sau đó khởi động lại Nginx:

`nginx -s reload`

hoặc 

`systemctl restart nginx`

Sau khi nhấn f5 nhiều lần sẽ hiển thị như sau: 

![Imgur](https://i.imgur.com/RlFOt1t.png)

Trên là cách cấu hình với 3 cách giới hạn số lượng request:
* Giới hạn với toàn bộ WebServer. 
* Giới hạn với 1 trang Website.
* Giới hạn với 1 đường dẫn xác định.