# Reverse Proxy Nginx
**Proxy server là gì?**
Là máy chủ trung gian hoặc giữa các máy chủ trung gian chuyển tiếp yêu cầu nội dung từ nhiều máy khách đến các máy chủ khác nhau trên Internet. Reverse Proxy Server là một loại máy chủ Proxy thường nằm sau firewall (tường lửa) trong privite network và hướng yêu cầu của máy khách đến máy chủ phụ trợ thích hợp. Nó cung cấp một mức độ kiểm soát bổ sung để đảm bảo luồng lưu lượng mạng troi chảy giữa máy khách và máy chủ.

Sử dụng một Reverse Proxy Server bao gồm:
* **Load balancing** - có thể hoạt động như một người điều hướng luồng đi, đứng trước các máy chủ phụ trợ của bạn và phân phối các nhóm yêu cầu của khách hàng qua một nhóm máy chủ theo các tối đa hóa tốc độ và sử dụng dụng lượng trong khi đảm bảo không có máy chủ nào bị quá tải. Có thể làm giảm hiệu suất. Nếu một máy chủ ngừng hoạt động, bộ cân bằng tải sẽ chuyển hướng lưu lượng đến các máy chủ trực tuyến còn lại.

* **Web acceleration**- Tăng tốc độ web: có thể nén dữ liệu không giới hạn, cũng như lưu trữ nội dung thường được yêu cầu, cả hai đều tăng tốc lưu lượng giữa máy khách và máy chủ. Ngoài ra nó cũng có thể được thực hiện các thao tác bổ sung như mã hóa SSL để tải xuống các máy chủ web của bạn, do đó tăng hiệu suất của chúng.

* **Security and anonymity**- Bảo mật và ẩn danh: Bằng cách chặn các yêu cầu hướng đến máy chủ phụ trợ của bạn, reverse proxy sẽ bảo vệ danh tính của họ và hoạt động như một biện pháp bảo vệ bổ sung chống lại các cuộc tấn công bảo mật. Nó cũng đảm bảo rằng nhiều máy chủ có thể được truy cập từ một trình định vị bản ghi hoặc URL bất kể các cấu trúc mạng local của bạn.

### Mô hình Proxy chuyển tiếp 
![](https://www.cloudflare.com/img/learning/cdn/glossary/reverse-proxy/forward-proxy-flow.svg)

Máy tính A sẽ tiếp cận trực tiếp với máy tính C, Khi máy A gửi request đến máy C, máy C sẽ responding cho máy A
### Mô hình Reverse Proxy Server 

![](https://www.cloudflare.com/img/learning/cdn/glossary/reverse-proxy/reverse-proxy-flow.svg)

Tất cả các yêu cầu từ D sẽ đươc gửi trực tiếp đến F và F sẽ gửi responding đến D. Với Reverse Proxy, tất cả các yêu cầu từ D sẽ được gửi trực tiếp đến E và E sẽ gửi yêu cầu của mình đến và nhận responding từ F. Sau đó E sẽ chuyển các câu trả lời thích hợp cho D

>**Kết luận**: Một Proxy chuyển tiếp nằm trước máy client và đảm bảo rằng không có máy chủ đích nào liên lạc trực tiếp với máy client cụ thể. Reverse Proxy nằm trước máy chủ đích và đảm bảo rằng không có máy client nào liên lạc trực tiếp với máy Server.
## Thực hành Với Reverse Proxy
Mô hình:

![Imgur](https://i.imgur.com/GhGa4EG.png)

IPPlanning:

!

Chuẩn bị giao diện cho 2 website, cài [LAMP](https://github.com/huydv398/Linux-tutorial/blob/master/CentOS-7/LAMP.md) hoặc [LEMP](https://github.com/huydv398/Linux-tutorial/blob/master/CentOS-7/LEMP.md) cho 2 web trên.

### Cấu hình cho Reverse Proxy Server

Chuẩn bị gói cài:

```
yum install -y update
yum install -y epel-release
```

Cài đặt và khởi động Nginx:

```
yum install nginx -y
systemctl start nginx
```

Nếu bạn đang chạy tường lửa, hãy chạy lệnh sau để cho phép lưu lượng HTTP và HTTPS:

```
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --reload
```

Tại thanh URL Kiểm tra Domain hoặc địa chỉ IP:
![Imgur](https://i.imgur.com/QKojA5q.png)

Mỗi site sẽ được khai báo tương úng với một file nằm trong thư mục `/etc/nginx/conf.d` Các file phải có phần đuôi mở rộng là `.conf`

### Site duonghuy.com

`vi /etc/nginx/conf.d/duonghuy.com.conf`

Thêm cấu hình cho file:

```
server {
    server_name duonghuy.com;
    client_max_body_size 1024M;

        location / {
            proxy_set_header X-Real-IP \$remote_addr;
            proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;
            proxy_set_header Host \$http_host;
            proxy_set_header X-NginX-Proxy true;
            proxy_pass http://10.10.1.3:80;
        }
}
```


### Site phucyen.net

`vi /etc/nginx/conf.d/phucyen.net.conf`

Thêm cấu hình cho file:

```
server {
    server_name phucyen.net;
    client_max_body_size 1024M;

        location / {
            proxy_set_header X-Real-IP \$remote_addr;
            proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;
            proxy_set_header Host \$http_host;
            proxy_set_header X-NginX-Proxy true;
            proxy_pass http://10.10.2.3:80;
        }
}
```

* `server_name phucyen.net`: Khi gọi tới tên miền `phucyen.net` nginx sẽ lấy dữ liệu từ `proxy_pass http://10.10.2.3:80`- địa chỉ ip:10.10.2.3 và port: 80.

Test và reload lại trạng thái cấu hình Nginx:
```
[root@nginx ~]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
[root@nginx ~]# nginx -s reload
```