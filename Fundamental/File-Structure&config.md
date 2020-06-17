# Tìm hiểu cấu trúc cấu hình và cấu hình tệp cấu hình Nginx.
Cấu trúc thư mục để xây dựng Nginx web server và VirtualHost

* Tất cả cấu hình nằm trong File thư mục:`/etc/nginx/`<br>Trong đó File `/etc/nginx/nginx.conf` là file cấu hình tổng quan trong hệ Nginx. Có thể sửa đổi để thay đổi cấu hình chung Nginx.
    * Thư mục:`/etc/nginx/sites-available/`- cấu các file cấu hình khối máy chủ trên mỗi trang web. Nginx chỉ có thể sử dụng khi kích hoạt khối tại thư mục `sites-enabled`.
    * Thư mục: `/etc/nginx/sites-enabled/`- Dùng để kích hoạt các khối máy chủ của mỗi trang web. Thông thường tạo liên kết file từ thư mục `sites-available` để dễ dàng quản lý và bật tắt size.

**Thư mục nội dung website**

Mặc định nó có thể nằm tại thư mục `/var/www/html/` hoặc thư mục `/usr/share/html`, nhưng bạn có thể thay đổi đường dẫn thư mục để quản lý hoặc bảo mật hơn khi sử dụng nhiều site trên cùng một server.

**Các thao tác cần thực hiện trước và sau khi chỉnh sửa file config.**

Trước khi thay đổi cấu hình, sao lưu lại file cấu hình:

`cp /etc/nginx/conf/nginx.conf /etc/nginx/conf/nginx.conf.backup`

Định kì sao lưu tập tin cấu hình nginx

Sau khi thực hiện thay đổi cấu hình trong file cấu hình, Restart lại Service.

`systemctl restart nginx`

>**Chú ý**
* Tất cả các dòng có chưa dấu `#` phía trước là những dòng chú thích (comment). Không có ý nghĩa với cấu lệnh.
* Các thiết lập được đặt trong ngoặc nhọn. Các ngoặc nhọn có thể lòng vào nhau cho nhiều khối lệnh.



## Tìm hiểu contexts cấu hình Nginx
Bài viết này bao gồm cấu trúc cơ bản được tìm thấy trong tệp cấu hình Nginx chính. Vị trị của tệp này sẽ thay đổi tùy thuộc vào cách bạn cài đặt phần mềm trên máy tính của mình. Đối với nhiều bản phân phối, tệp sẽ được đặt tại `/etc/nginx/nginx.conf`. Nếu nó không tồn tại ở đó thì nó cũng có thể ở `/usr/local/nginx/nginx.conf` hoặc `usr/local/etc/nginx/nginx/conf`.

Điều đầu tiên cần chú ý khi xem tệp cấu hình chính là nó dường như được tổ chức theo cấu trúc giống như root, được xác định bởi các bộ dấu ngoặc '`{` `}`'. Theo cách nói của Nginx, các khu vực mà các dấu ngoặc này xác định được gọi là bối cảnh của web vì chúng chứa các chi tiết cấu hình được phân tách theo khu vực quân tâm của chúng. Về cơ bản, có bộ phận này cung cấp một cấu trúc tổ chúc cùng với một số logic có điều kiện để quyết định có áp dụng các cấu hình bên trong hay không.

Vì các bối cảnh có thể được xếp lớp trong nhau, Nginx cung cấp mức độ kế thừa chỉ thị. Theo nguyên tắc chung, nếu một lệnh có giá trị trong nhiều phạm vi lồng nhau, một khai báo trong ngữ cảnh rộng hơn sẽ được chuyển cho bất kỳ bối cảnh con nào làm giá trị mặc định. Điều đáng chú ý là việc ghi đè lên bất kỳ chỉ thị kiểu mảng nào sẽ thay thế giá trị trước đó, không nối vào nó

Chỉ thị có thể được sử dụng trong bối cảnh mà chúng được thiết kế cho Nginx. Nginx sẽ báo lỗi khi đọc tệp cấu hình với các lệnh được khai báo trong ngữ cảnh sai.

**The Core contexts**

The first group of contexts is The Core contexts that nginx use để create tree phân cấp.

**The main Context**
Cũng có thể gọi là Global context. Đây là context tổng quát nhất bao gồm tất cả các directive đơn giản, block directive và các context khác.

Về nội dung config file của nginx:

```
#user  nobody;
worker_processes  1;

#error_log  /var/log/nginx/error.log;
#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}
    }

}
```
File bắt đầu cùng với 4 chị thị(directives): `user`, `worker_processes`, `error_log` và `pid`. Chúng nằm ngoài bất kỳ block hay context cụ thể nào do dố cúng nằm trong Main Vontext.

**The Events Context** : Được sử dụng để đặt các tùy chọn global ảnh hưởng đến cách Nginx xử lý các kết nối ở mức chung. Chỉ có thể có một bối cảnh sự kiện duy nhất được xác định trong cấu hình **Nginx**:

```
# main context

events {
    # events context
    . . .
}
```

Nginx sử dụng mô hình xử lý kết nối dựa trên *events*, do đó các lệnh được xác định trong context được xác định cách các quy trình làm việc nên xử lý cách kết nối. Context được tìm thấy ở đây được sử dụng để chọn kỹ thuật xử lý kết nối sẽ sử dụng hoặc để sửa đổi cách thức thực hiện các phương thức này.

Thông thường, Phương thức xử lý kết nối được chọn tự động dựa trên sự lựa chọn hiệu nhất mà nền tảng có sẵn. Đối với các hệ thống Linux, Phương pháp `epoll` thường được lựa chọn.

Các mục khác có thể được định cấu hình là số lượng kết nối mà mỗi công việc có thể xử lý, cho dù một công việc sẽ chỉ thực hiện một kết nối duy nhất tại thời điểm hoặc thực hiện tất cả các kết nối đang chờ xử lý sau khi được thông báo về kết nối đang chờ xử lý và liệu các công việc sẽ lần lượt phản ứng với *events*.

**Bối cảnh HTTP**: Khi định cấu hình làm web server hoặc reverve proxy, context của http sẽ giữ phần lớn. Bối cảnh này sẽ chứa tất cả các chỉ thị và các context khác cần thiết để xác định cách chương trình sẽ xử lý các kết nối HTTP hoặc HTTPS.

**HTTP Context** gần giống với **Events Context**, vì vậy chúng được liệt kê cạnh nhau, thay vì lồng nhau. Cả hai đều là con của **Main Context**:

```
#main context

events {
    #events context
    . . .
}

http {
    # htttp context
}
```

Mặc dù CONTEXT thấp hơn sẽ cụ thể hơn về cách xử lý các yêu cầu, các chỉ thị ở cấp độ này kiểm soát các mặc định cho mọi máy chủ ảo được xác định bên trong. Một số lượng lớn các lệnh có thể được cấu hình trong context này bên dưới, tùy thuộc vào cách bạn muốn kế thừa hoạt động.

Một số các chỉ thị rằng bạn có khả năng kiểm soát cuộc gặp gỡ các vị trí mặc định 
* truy cập và lỗi bản ghi(`access_log` và `error_log`).
* Cấu hình không đồng bộ bật tắt cho các hoạt động tập tin( `aoi`, `sendfile` và `directio` ).
* Cấu hình trạng thái máy chủ khi xảy ra lỗi(`error_page`).
* Chỉ thị cấu hình nén (`gzip` và `gzip_disable`).
* Tinh chỉnh TCP giữ các thiết lập còn hoạt động(`keeplive_disable`, `keepalive_requests` và `keepalive_timeout`). 
* Các quy tắc mà **Nginx** sẽ đi theo để cố gắng gói tối ưu hóa và các cuộc gọi hệ thống(`sendfile`, `tcp_nodelay` và `tcp_nopush`).
* Các chỉ thị bổ sung định cấu hình tệp chỉ thư mục và gốc tài liệu ở cấp ứng dụng (*root* và *index*) và thiết lập các bằng hash khác nhau( `*_hash_bucket_size` và `*_hash_max_size` cho Server_names, types và variables ).

**Server contexts**: được khai báo trong http context. Là context lồng nhau. Được đạt trong ngoặc. Context cho phép khai báo nhiều lần.

```
# main context
http {
    # http context

    server {
        #first server context
    }

    server {
        # second server context 
    }
}

```

Lý do cho phép nhiều khai báo về **Server Context** là mỗi trường hợp xác định một máy chủ ảo cụ thể để xử lý các yêu cầu của máy khách. Bạn có thể có nhiều khối máy chủ. Mỗi khối có thể xử lý một tập hợp con cụ thể của các kết nối.

Do khả năng của nhiều máy chủ, Context này cũng là lần đầu tiên Nginx phải sử dụng thuật toán lựa chọn để đưa ra quyết định. Mỗi yêu cầu máy khách sẽ được sử lý theo  cấu hình được xác định trong một ngữ cảnh máy chủ, do đó Nginx phải quyết định bối cảnh máy chủ nào phù hợp nhất dựa trên chi tiết của yêu cầu. Các chỉ thị quyết định nếu một khối máy chủ sẽ được sử dụng yêu cầu :

* `Listen`: sự kết hợp địa chỉ IP/ port mà khối máy chủ này được thiết kế để đáp ứng. Nếu một yêu cầu được thực hiện bởi một khách hàng phù hợp với các giá trị này, khối này sẽ có khả năng được chọn để xỷ lý kết nối.

* `server_name`: Lệnh này thành phần khác được sử dụng để chọn khối máy chủ để xử lý. Nếu có nhiều thành phần khối máy chủ có chỉ thị lắng nghe có cùng độ đặc hiệu có thể xử lý yêu cầu, Nginx sẽ phân tích tiêu đề của máy chủ lưu trữ tren máy chủ của yêu cầu và khớp với chỉ thị này.

Các lệnh trong ngữ cảnh này có thể ghi đè nhiều lệnh có thể được xác định trong ngữ cảnh http, bao gồm ghi logging, document root,compression(nén), etc ...Ngoài các lệnh được lấy từ ngữ cảnh http, chúng tôi có thể định cấu hình tệp để cố gắng trả lời yêu cầu(`try_file`), đưa ra các chuyển hướng và viết lại (`return`) và (`rewrite`) và đặt các biến tùy ý (`set`).

`Location context`: chia sẻ nhiều phẩm chất quan hệ với bối cảnh máy chủ. ví dụ nhiều bối cảnh vị trí có thể được xác định, mỗi vị trí được sử dụng để xứ lý một loại yêu cầu khách hàng nhất định và mỗi vị trí được chọn nhờ phù hợp với định nghĩa vị trí so với yêu cầu khách hàng thông qua thuật toán lựa chọn.

Mặc dù các chỉ thị xác định có chọn khối máy chủ được xác định trong ngữ cảnh máy chủ hay không, thành phần quyết định khả năng sử lý yêu cầu của vị trí được đạt trong định nghĩa vị trí:
```
location match_modifier location_match {

    . . .

}
```
*  match_modifier: Bạn có thể hiểu là so sánh để tìm ra đối chiếu với location_match
* ví dụ
```
location /site{
    . . .
}
```
Các Request có dạng URI có dạng như sau: `/site`, `/site/login.index`, `/site/page/theme/our` location sẽ sử lý để tìm đến thư mục /site/ trên server để hiện thị nội dung.

Với khai báo trên thì chỉ có `/site` có thể được sử lý, nhưng file `/site/index.html` và `/site/index.php` thì không.

```
location ~ \.(jpeg|png|gif|ico)$ {

. . .

}
```
Các yêu cầu có đuôi là `.jpeg`, `.png`, `.gif`, `.ico` có thể xử lý để hiện thị.

Điều này có thể hữu ích cho việc tạo bối cảnh vị trí tổng quát hơn và sau đó xử lý thêm dựa trên các tiêu chí cụ thể hơn với bối cảnh bổ sung bên trong:
```
# main context

server {

    # server context

    location /match/criteria {

        # first location context

    }

    location /other/criteria {

        # second location context

        location nested_match {

            # first nested location

        }

        location other_nested {

            # second nested location

        }

    }

}
```

Các khối vị trí sống trong bối cảnh máy. Không giống các khối máy chủ, có thể được lồng vào nhau. Điều này có tể hữu ích cho việc tạo bối cảnh vị trí tổng quát hơn để bắt một tập hợp lưu lượng truy cập nhất định và sau đó xử lý thêm dựa trên các tiêu chí cụ thể hơn với bối cảnh bổ sung ben trong:
```
# main context

server {

    # server context

    location /match/criteria {

        # first location context

    }

    location /other/criteria {

        # second location context

        location nested_match {

            # first nested location

        }

        location other_nested {

            # second nested location

        }

    }

}
```
Thông thường khi mà location block được dùng để phục vụ một request nào đó thì action sẽ hoàn toàn nằm trong context của nó. Và nó sẽ chỉ nhảy sang các block khác hay chuyển hướng xử lý request khi có yêu cầu từ chính bên trong context của nó. Một vài chỉ thị có thể yêu cầu:
```
- index
- try_files
- rewrite
- error_page
```
***index directive***
Nằm bên trong location luon được nginx trỏ tới trang đầu tiền khi xử lý điều hướng yêu cầu. Chỉ rõ các file hoặc chính xác tên và nó sẽ không phát sinh khi có tên mới.

```
location = / {
    index index.html;
}
```

***try_files directive***
Cố gắng phục vụ một tệp tin được chỉ rõ(các tham số), nếu không có tệp tin nào tồn tại, nhảy đến khối location được khai báo(tham số cuối cùng trong chỉ thị) hoặc phục vụ 1 URI được chỉ định
```
location / {
    try_files $uri $uri.html $uri/ /fallback/index.html;
}
```
***rewrite directive***

Khác với Apache, Ngĩn không sử dụng file *.htaccess* nên khi bạn cần rewrite Url sẽ phải convert qua rule Nginx.
```
location /download/ {
    rewrite ^(/download/.*)/media/(.*)\..*$ $1/mp3/$2.mp3 break;
    rewrite ^(/download/.*)/audio/(.*)\..*$ $1/mp3/$2.ra  break;
    return  403;
}
```

***error_page directive***
Chỉ thị khi không tìm thấy file tham chiếu.
```
location / {
    error_page 404 = @fallback;
}

location @fallback {
    proxy_pass http://backend;
}
```

* Xử lý trong `location context` 
    * Nginx sẽ đọc `root` directive để xác định thư mục chứa trang client yêu cầu. Thứ tự các trang được ưu tiên sẽ được khai báo trong chỉ thị index.
    * Nếu không tìm thấy nội dung mà client yeu cầu, Nginx sẽ điều hướng sang Location context khác và thông báo lỗi cho người dùng

ví dụ 1:
```
location / {
root html;
index index.html index.htm;
}
```
rooi trỏ tới thư mục `html/`. Trong cài đặt mặc định của Nginx, đường dẫn đầy đủ đến thư mục này là `/etc/nginx/html`
**Request**: [http://demo.com/blog/style.css]()
**Returns**: NGINX sẽ tìm yêu cầu của client theo đường dẫn /etc/nginx/html/blog/style.css
 Mặc dù bối cảnh máy chủ được chọn dựa trên tổ hợp địa chỉ IP/ port được yêu cầu máy chủ trong tiêu đề của máy chủ lưu trữ, các khối vị trí tiếp tục phân chia xử lý yêu cầu trong khối máy chủ bằng cách xem URI yêu cầu. URI yêu cầu  là một phần của yêu cầu xuất hiện sau khi kết hợp tên miền hoặc địa chỉ IP /port 

**The Upstream context**: được sử dụng để xác định và cấu hình các máy chủ upstream của web. Về cơ bản, bối cảnh này xác định một nhóm máy chủ được đặt tên mà nginx sau đó có thể yêu cầu proxy. context này có thể sẽ được sử dụng khi bạn định cấu hình các loại proxy.

context nên được đặt trong bối cảnh http, bên ngoài bất kỳ context máy chủ cụ thể nào.
```
# main context

http {

    # http context

    upstream upstream_name {

        # upstream context

        server proxy_server1;
        server proxy_server2;

        . . .

    }

    server {

        # server context

    }

}
```

Sau đó, có thể được tham chiều theo tên trong các khối máy chủ hoặc vị trí để chuyển các yêu cầu của một loại nhất định đến nhóm náy chủ đã được xác định. Sau đó, sẽ sử dụng một thuật toán để xác định máy chủ cụ thể nào sẽ gửi yêu cầu. Bối cảnh này cung cấp cho Nginx khả nagnw thực hiện một số cân bằng tải khi yêu cầu ủy quyền.

**The Mail context**: mặc dù Nginx thường được sử dụng làm web server hoặc reverse proxy server, nó cũng có thể hoạt động như một mail proxy server hiệu suất cao. context được sử dụng cho các chỉ thị loại này được gọi một cách thích hợp là thư mail. mail context được xác định trong bối cảnh chính tren toàn global hoặc main.

Chức năng chính của bối cảnh là thư cung cấp một khu vực để dịnh cấu hình giải pháp ủy quyền thư trên máy chủ. Nginx có khả năng chuyển hướng yêu cầu xác thực đến một máy chủ xác thực bên ngoài. nó có thể truy cập vào các máy chủ mail POP3 và IMAP đê phục vụ dữ liệu thực tế. cũng được cấu hình để kết nối với SMTP Relayhost nếu muốn

```
# main context

mail {

    # mail context

}
```

**The If Context**: Có thể được thiết lập để cung cấp xử lý có điều kiện các chỉ thị được xác định trong. Giống như một câu lệnh if trong lập trình thông thường, lệnh if trong nginx sẽ thực hiện các lệnh được chứa nếu một thử nghiệm đã trã về cho đúng true

**The Limit_except Context**: Được sử dụng để hạn chế việc sử dụng các phương thức HTTP nhất định trong một số bối cảnh địa điểm. ví dụ nếu chỉ một số khách hàng nhất định sẽ có quyền truy cập vào nội dung POST, nhưng mọi người đều có khả năng đọc nội dung, bạn có thể sử dụng khối Limit_except để các định yêu cầu này.

```
# server or location context

location /restricted-write {

    # location context

    limit_except GET HEAD {

        # limit_except context

        allow 192.168.1.1/24;
        deny all;
    }
}
```

Điều này sẽ áp dụng các chỉ thị bên trong ngữ cảnh(có nghĩa là hạn chế quyền truy cập) khi gặp bất kì phương thức HTTP nào ngoại trừ các phương thúc được liệt ke trong tiêu đề context. Kết quả của ví dụ tren là bất kỳ máy khách nào cũng có thể sử dụng nọi dung động từ GET và HEAD, nhưng chỉ những máy khách đến từ subnet `192.168.1.1/24` mới được phép sử dụng các phương thức khác.

