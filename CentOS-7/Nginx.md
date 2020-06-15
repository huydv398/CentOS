# Cài đặt Nginx lên CentOS-7

## Điều kiện
Trước khi bắt đầu
* Thực hiện lệnh dưới quyền root.
* Máy thực hiện phải có Internet

## Thực hiện cài đặt
Thêm kho lưu trữ Nginx,

`rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm`

Tải xuống và cài đặt Nginx:

`yum install -y nginx`

Trong hướng dẫn sẽ hướng dẫn tạo một khối máy chủ cho `demo1.com` và một khối cho `demo2.com`. Nên thay thế tên miền hoặc giá trị riêng của bạn khi làm theo.

**Bước một - Tạo cấu trúc thư mục**

Trong mỗi thư mục này, tạo một thư mục html với flag -c cho phép tạo một thư mục lồng bên trong nó:

```
mkdir -p /var/www/huydv.com/html
mkdir -p /var/www/huyts.com/html
```

#### Cấp quyền 
Cấu trúc thư mục được sở hữu bởi người dùng *root*, nếu muốn người dùng thường xuyên có thể sửa đổi các tệp trong thư mục web, thay đổi quyền sở hữu với `chown`:

```
 chown -R $USER:$USER /var/www/huyts.com/html/
chown -R $USER:$USER /var/www/huydv.com/html/
```

Các biến `$USER` sẽ mất giá trị giá trị của người dùng mà bạn đăng nhập bằng lệnh. Bằng cách này, người dùng thông thường hiện sở hữu các thư mục con `public_html` nơi sẽ lưu trữ nội dung.

Nên sửa đổi các quyền để đảm bảo rằng quyền truy cập đọc được phép vào thư mục web chung và tất cả các tệp và thư mục bên trong, để các trang có thể phục vụ chính xác:

`chmod -R 755 /var/www`

Máy chủ web của bạn bây giờ sẽ có các quyền cần thiết để phân phát nội dung và người dùng của bạn sẽ có thể tạo nội dung trong các thư mục phù hợp

## Tạo trang demo cho mỗi trang web

Tạo một trang `index.html` cho mỗi trang web xác định.

`vi /var/www/huydv.com/html/index.html`

Trong tệp này, tạo một tài liệu HTML đơn giản cho web:

```
<html>
  <head>
    <title>Welcome to huydv.com!</title>
  </head>
  <body>
    <h1>Success! Thanh cong</h1>
  </body>
</html>
```

Lưu và đóng tệp khi hoàn thành.

Có thể sao chép tệp này để sử dụng làm mẫu cho trang web thứ hai:

`cp /var/www/huydv.com/html/index.html /var/www/huyts.com/html/index.html`


Mở tệp vừa tạo và sửa đổi các thông tin có liên quan:
```
<html>
  <head>
    <title>Welcome to huyts.com!</title>
  </head>
  <body>
    <h1>Success! Thanh cong</h1>
  </body>
</html>
```

Lưu và đóng tập tin này. Bây giờ bạn có các trang cần thiết để kiểm tra cấu hình khối máy chủ


## Tạo File khối Server mới.

Các File khối Server là những gì chỉ định cấu hình của các trang web riêng biệt và chỉ ra cách để máy nginx sẽ đáp ứng các yêu cầu tên miền khác nhau.

Để bắt đầu, thiết lập thư mục mà các khối máy chủ sẽ được lưu trữ, cũng như cho thư mục Nginx biết rằng một khối máy chủ đã sẵn sàng phục vụ cho khách hàng truy cập. Thư mục `site-available` sẽ giữ tất cả các tệp khối máy chủ, trong khi thư mục `site-enabled` sẽ giữ các liên kết tượng trưng đến các khối máy chủ mà muốn xuất bản. Tạo cả hai thư mục bằng cách:

```
mkdir /etc/nginx/sites-available
mkdir /etc/nginx/sites-enabled
```

>**Lưu ý**: Bố cục thư mục này được giới thiệu bởi những người đóng góp Debian, Cách này để tăng tính linh hoạt với việc quản lý các khối máy chủ (vì việc tạm thời bật tắt các khối máy chủ theo cách này dễ dàng hơn).

Tiếp theo, Nói với Nginx để tìm các khối máy chủ trong thư mục `sites-enabled`. Để thực hiện điều này, chỉnh sửa tệp cấu hình chính của Nginx và thêm một dòng khai báo một thư mục tùy chọn cho các tệp cấu hình bổ sung:

`vi /etc/nginx/nginx.conf`

Thêm các dòng này vào cuối khối `http {}`:

```
include /etc/nginx/sites-enabled/*.conf;
server_names_hash_bucket_size 64;
```

Dòng đầu tiên hướng dẫn Nginx tìm kiếm các khối máy chủ trong thư mục `sites-enabled`, trong khi dòng thứ hai tăng dung lượng bộ nhớ được phân bổ để phân tích tên miền(vì hiện đang sử dụng nhiều tên miền).

Khi bạn hoàn thành việc thực hiện những thay đổi này, bạn có thể lưu và đóng tệp. Bây giờ đã sẵn sàng để tạo tập tin khối máy chủ.

### Tạo tên khối máy chủ đầu tiên
Theo mặc định, Nginx chứa một khối máy chủ được gọi mẫu `default.conf` mà có thể sử dụng làm mẫu cho các cấu hình riêng của mình. Có thể tạo tệp cấu hình khối máy chủ đầu tiên bằng cách sao chép tệp mặc định:

`cp /etc/nginx/conf.d/default.conf /etc/nginx/sites-available/huydv.com.conf`

Bây giờ, mở tệp mói trong trình soạn thảo của bạn với quyền root:

`vi /etc/nginx/sites-available/huydv.com.conf`

>**Lưu ý**: Tất cả các tệp khối máy chủ phải kết thúc `.conf`

Bỏ qua các dòng nhận xét tệp sẽ được hiển thị như sau:

```
[root@server1 conf.d]# egrep -v  "^#|^*#|^$"  huydv.conf
server {
    listen       80;
    server_name  localhost;
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}

```

* `Server_name`: điều này cho Nginx biết yêu cầu trỏ đến khối máy chủ này. Khai báo tên máy chủ chính `huydv.com`, cũng như một bí danh bổ sung `www.huydv.com`, sao cho cả `www.` và không `www.` yêu cầu cung cấp cùng một nội dung

`server_name huydv.com www.huydv.com;`

>**Lưu ý**: Mỗi câu lệnh Nginx phải kết thúc bằng dấu chấm phẩy(`;`), vì vậy hãy kiểm tra từng dòng câu lệnh của bạn nếu bạn gặp vấn đề sau này

* `root`: Trỏ tới gốc tài liệu của trang mà bạn đã tạo:

`root /var/www/huydv.com/html;`


Thêm một lệnh `try_files`, lệnh kết thúc bằng lỗi 404 nếu không tìm thấy tên tệp hoặc thư mục mong muốn:

`try_files $uri $uri/ =404;`

Khi bạn kết thúc, tệp sẽ được như sau:
```
[root@server1 sites-available]# egrep -v "^#|^*#|^$" huydv.com.conf
server {
    listen       80;
    server_name  huydv.com www.huydv.com;
    try_files $uri $uri/ =404;
    location / {
        root   /var/www/huydv/html;
        index  index.html index.htm;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

### Tạo tệp khối máy chủ thứ 2

Đã thiết lập tệp khối máy chủ đầu tiên, tạo tệp thứ hai bằng cách sao chép tệp và điều chỉnh nếu cần.

`cp /etc/nginx/sites-available/huydv.com.conf /etc/nginx/sites-available/huyts.com.conf`

Mở tệp mới với quyền root trong trình soạn thảo:

`vi /etc/nginx/sites-available/huyts.com.conf`

Bây giờ bạn cần sửa đổi tất cả phần thông tin để tham chiếu tên miền thứ 2.

Sau khi chỉnh sửa hoàn tất thông tin, tệp khối máy chủ thứ 2 sẽ như sau:
```
[root@server1 conf.d]# egrep -v "^#|^*#|^$" /etc/nginx/sites-available/huyts.com.conf
server {
    listen       80;
    server_name  huyts.com www.huyts.com;
    try_files $uri $uri/ =404;
    location / {
        root   /var/www/huyts.com/html;
        index  index.html index.htm;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}

```

## Kích hoạt tệp khối máy chủ mới

Kích hoạt để Nginx biết để phục vụ cho khách hàng truy cập. Tạo một liên kết tượng trương cho từng khối máy chủ trong thư mục `sites-enabled`:

```
ln -s /etc/nginx/sites-available/huydv.com.conf /etc/nginx/sites-enabled/huydv.com.conf
ln -s /etc/nginx/sites-available/huyts.com.conf /etc/nginx/sites-enabled/huyts.com.conf
```

Khi kết thúc, Khởi động lại Nginx để những thay đổi này có hiệu lực:

`systemctl restart nginx`

## Thiết lập tệp Local Hosts

Nếu bạn đã sử dụng các tên miền ví dụ thay vì các tên miền thực tế để kiểm tra quy trình này, bạn vẫn có thể kiểm tra chúc năng của của khối máy chủ của mình bằng các tạm thời sửa đổi file `hosts tên máy cục bộ của bạn.

Nếu bạn đang sử dụng máy Window 10, Truy cập vào file có đường dẫn `C:\Windows\System32\drivers\etc\hosts` sửa với quyền của quản trị viên

Ví dụ, Tôi đang thực hành trên máy ảo VMware được cài trên Win 10,được cài Server CensOS-7 có địa chỉ IP: `192.168.20.3`

Sửa file với cấu trúc sau

```
#Ví dụ

192.168.20.3    huyts.com
192.168.20.3    huydv.com
```

Điều này sẽ chỉ dẫn bất kỳ yêu cầu nào cho huydv.com và huyts.com trên máy tính local của tôi và gửi chúng đến máy chủ tại server có IP: `192.168.20.3`

## Kiểm tra kết quả của bạn

