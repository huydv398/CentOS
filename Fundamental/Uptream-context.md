# Defining an Upstream Context for Load Balancing Proxied Connections
## Xác định Upstream Context cho các kết nối cân bằng tải.

Nginx cho phép dễ dàng mở rộng cấu hình bằng cách chỉ định toàn bộ nhóm máy chủ **backend** mà có thể chuyển yêu cầu *request*.

Sử dụng **Upstream Context** để xác định nhóm máy chủ. Cấu hình này giả định rằng bất kỳ một trong các máy chủ được liệt kê đều có khả năng sử lý yêu cầu khách hàng. Điều này cho phép mở rộng quy mô cơ sở hạ tầng mà mà không quá khó khăn. Lệnh `Upstream` phải được đặt trong **http context** của cấu hình *nginx* của bạn. 

Dưới đây là ví dụ đơn giản:

```
# http context

upstream backend_hosts {
    server host1.example.com;
    server host2.example.com;
    server host3.example.com;
}

server {
    listen 80;
    server_name example.com;

    location /proxy-me {
        proxy_pass http://backend_hosts;
    }
}

```

Trong ví dụ trên, thiết lập một **Upstream Context** được gọi là `backend_hosts`. Sau khi xác định được, tên này sẽ có sẵn để sử dụng trong các lần ủy quyền như thể đó là một tên miền thông thường. Trong **server block** chuyển bất kỳ yêu cầu được gửi đến `example.com/proxy-me/...`đến nhóm đã được xác định ở trên. Trong nhóm đó, một máy chủ được chọn bằng cách áp dụng thuật toán có thể định cấu hình. Theo mặc định, đây chỉ là một quy trình lựa chọn vòng tròn đơn giản(Mỗi yêu cầu sẽ lần lượt chuyển đến một máy chủ khác nhau).

## Thay đổi thuật toán load balancing

Bạn có thể thay đổi thuật toán **Load Balancing** được sử dụng bởi nhóm **backend** bằng cách bao gồm các (directives)-lệnh hoặc cờ trong **upstream context**:

* **(round robin)**- vòng tròn: **Thuật toán cân bằng tải mặc định được sử dụng** nếu không có **chỉ thị cân bằng(balancing directives)** nào khác. Mỗi máy chủ được xác định trong the **Upstream Context** được chuyển lần lượt các *request*.
* **least_conn**: Chỉ định rằng các kết nối mới phải luôn được cung cấp cho **backend** có số lượng kết nối hoạt động ít nhất. Điều này có thể đặc biệt hữu ích trong các tình huống mà các kết nối đến phụ trợ có thể tồn tại trong một thời gian.

* **Ip_hash**: Thuật toán cân bằng này phân phối các yêu cầu đến các máy chủ khác nhau dựa trên địa chỉ IP của máy khách, Ba octec đầu tiên được sử dụng làm chìa khóa để quyết định máy chủ xử lý yêu cầu. Kết quả là các máy khách có xu hướng được phục vụ một máy chủ mỗi lần, điều này có thể hỗ trợ tính nhất quán của phiên.

* **Hash**: Thuật toán cân bằng này chủ yếu được sử dụng với **proxy memcached**. Các máy chủ được chia dựa trên giá trị của khóa **hash** được cung cấp tùy ý. Đây có thể là văn bản, biến hoặc một tổ hợp. Đây là phương pháp duy nhất yêu cầu người dùng cung cấp dữ liệu, đây là chìa khóa nên được sử dụng cho hàm **hash**.

Khi thay đổi thuật toán cân bằng, Khối có thể như sau:

```
# http context

upstream backend_hosts {

    least_conn;

    server host1.example.com;
    server host2.example.com;
    server host3.example.com;
}
```

Trong ví dụ trên, máy chủ sẽ được chọn dựa trên cái nào có ít kết nối nhất. Lệnh `ip_hash` này có thể được đặt theo cùng một cách để có được một số phiên nhất định.

Đối với phương thức `hash`, bạn phải cung cấp khóa key để `hash`. Đây có thể là bất cứ điều gì bạn muốn:
```
# http context

upstream backend_hosts {

    hash $remote_addr$remote_port consistent;

    server host1.example.com;
    server host2.example.com;
    server host3.example.com;
}

. . .
```

Ví dụ trên sẽ phân phối các yêu cầu dựa trên giá trị của địa chỉ IP máy khách và port. Thêm tham số tùy con `consitent`, thực hiện thuật toán `hash` nhất quán về ketama. Về cơ bản, điều này có ý nghĩa là nếu các upstream servers thay đổi, sẽ có tác động tối thiểu đến bộ nhớ cache của bạn.

## Setting Server Weight for Balancing

Theo khai báo của các máy chủ phụ trợ, theo mặc định, mỗi máy chủ đều có trọng số tương đương với nhau. Điều này giả định rằng mỗi máy chủ có thể và nen xử lý cùng một lượng tải(có tính đến các tác đọng của các thuật toán cân bằng). Tuy nhiên, bạn cũng có thể đặt **weight** số thay thế cho các máy chủ trong khi khai báo:

```
# http context

upstream backend_hosts {
    server host1.example.com weight=3;
    server host2.example.com;
    server host3.example.com;
}

. . .
```

Trong ví dụ trên, `host1.example.com` sẽ nhận được ba lần lưu lượng như hai máy chủ khác. Theo mặc định, mỗi máy chủ được gán một  số **weight**.



