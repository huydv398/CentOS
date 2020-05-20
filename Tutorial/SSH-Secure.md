# SSH - Secure Shell 

## 1) Khái niệm
* **SSH - Secure Shell** là một giao thức mạng dùng để thiết lập kết nối mạng một cách bảo mật.
* **SSH** có sử dụng cơ chế mã hóa đủ mạnh nhằm ngăn chăn các hiện tượng nghe trộm, đánh cắp thông tin trên đường truyền. Các giao thức trước đây là telnet không hỗ trợ mã hóa. 
* **SSH** hoạt động ở tầng **Application** trong mô hình **TCP/IP**
* Các công cụ **SSH** ( như là **OPenSSH**,..)cung cấp cho người dùng cách thức để thiết lập kết nối mạng được mã hóa để tạo một kênh kết nối riêng tư.

## Tổng quan
Xác thực khóa công khai là cách đăng nhập vào tài khoản **SSH**/**SFTP** bằng khóa mật mã thay vì mật khẩu.

Cung cấp nhiều lợi ích khi làm việc với nhà phát triển. Với SSH key bạn có thể:
* Cho phép nhiều nhà phát triển đăng nhập với cùng một người dùng hệ thống mà không phải chia sẻ một mật khẩu duy nhất giữa họ;
* Thu hồi quyền truy cập của một nhà phát triển mà không thu hồi quyền truy cập của nhà phát triển khác;
* Giúp nhà phát triển đơn lẻ đăng nhập vào nhiều tài khoản hơn mà không cần quản lý nhiều mật khẩu khác nhau.
 

## Các kỹ thuật mã hóa
* Một lợi thể quan trọng của **SSH** so với các giao thức tiền nhiệm trước đó là sử dụng mã hóa để đảm bảo kết nối bảo mật giữa **host** và **client**
    * **Host** là server đầu xa.
    * **Client** là máy tính đang thực hiện truy cập từ xa vào **host** 
* Có 3 công nghệ mã hóa khác nhau được sử dụng trong **SSH** là : ***symmetric encryption***, ***asymmetric encryption***, ***hashing*** .

### Symmetric encryption (Mã hóa đối xứng)
* Là một dạng mã hóa được sử dụng secret key cho cả việc mã hóa và giải mã gói tin bởi host và client. Do vậy, bất cứ ai có được secret key đều có thể giải mã gói tin.
* symmetric encryption thường được gọi là shared-key hoặ shared-secret encryption. Thường chỉ có 1 key được sử dụng hoặc đôi khi là 1 cặp key trong khi từ key này có thể tính toán ra được kia kia.
* **Symmetric keys** được sử dụng để mã hóa toàn bộ giao tiếp giữa 2 máy trong suốt phiên **SSH**. Cả **server** và **client** truy gốc **secret key** bằng việc sử dụng 1 *thuật toán đồng nhất* , và **key** sau khi được tổng hợp sẽ không được tiết lộ với bất kỳ bên thứ 3 nào. Quá trình tạo ra **symmetric key** được tiến hành bởi *thuật toán đổi key mã hóa* (**key exchange algorithm**). Điều làm cho thuật toán này đặc biệt ăn toàn là **key** không bao giờ được trao đổi giữa **client** và **host**. thay vào đó, 2 máy tính trao đổi 1 phần thông tin chung và sau đó thực hiện tính toán độc lập ra **secret key**. Kể cả khi có 1 máy khác bắt được phần thông tin này, nó cũng có thể tính toán được **secret key** bởi không biết thuật toán trao đổi **key**
* Có rất nhiều thuật toán mã hóa(**cypher**) tồn tại, bao gồm **AES** (**Advanced Encryyption Standard**), **CAST128** , **Blowfish**, ... Trước khi thiết lập một kết nối bảo mật, **Client** va **server** sẽ quyết định xem dùng thuật toán mã hóa nào bằng cách xuất ra 1 danh sách các thuật toán mã hóa được hỗ trợ theo thứ tự ưu tiên. Thuật toán được ưu tiên nhất từ phía **Client** và có xuất hiện trong danh sách thuật toán hỗ trợ của **host** sẽ được sử dụng làm thuật toán mã hóa chung

### 2) Asymmetric Encryption (mã hóa bất đối xứng)
* Không giống **symmetric encryption**, **asymmetric encryption** sử dụng 2 **key** riêng biệt để mã hóa và giải mã. Hai loại **key** được biết đến với tên gọi là **public key** và **private key**. Cùng với nhau chúng tạo thành 1 cặp key hay được gọi là **key-pair**.
* **Public key** được *phân phối* và *chia sẻ* với các bên khác. Trong khi nó được liên kết với **private key** theo một cách nào đó, **private key** không thể bị tính toán ra từ **public key**. Mối quan hệ giữa hai loại **key** này khá phức tạp: gói tin được mã hóa bởi **Public key** của máy nào thì chỉ giải mã được chính **private** của nó. Mối quan hệ 1 chiều này có nghĩa **Public key** không thể giải mã gói tin của chính nó, và cũng không thể giải mã bất cứ thứ gì được mã hóa bởi **private key**.
* *Private key* sẽ ở lại với máy chủ nó để kết nối được bảo mật, sẽ không có bên thứ 3 nào biết nó.
* Điều làm cho kết nối được an toàn giữa 2 máy làm việc **Private key** sẽ không bao giờ bị lộ, đó là khả năng duy nhất để giải mã gói tin sử dụng Public key của nó. Do đó, bất kỳ bên nào có khả năng giải mã gói tin Public phải sở hữu private key tương ứng.
* Asymmetric encryption không được sử dụng mã hóa toàn bộ phiên SSH. Thay vào đó, nó chỉ được sử dụng trong thuật toán trao đổi key của symmetric encryption. Trước khi khởi tạo 1 kết nối bảo mật, cả hai bên tạo ra 1 cặp public-private keys tạm thời , sau đó chia sẻ private keys của chúng để tạo ra shared secret key.
* Trong quá trình symmetric được thiết lập, server sử dụng public key của client để tạo ra và thử thách và truyền nó tới client để xác thực. Nếu client có thể giải mã gói tin thành cộng, có nghĩa là nó giữ private key được yêu cầu cho kết nối.

=> Tới đâu phiên làm việc SSH được bắt đầu.
### 3) Hashing 
* Hashing một chiều 1 dạng mã hóa khác được sử dụng bởi SSH.
Chức năng của chúng khác với 2 dạng mã hóa trên theo hưởng rằng chúng sẽ không bao giờ bị giải mã.
* Chúng tạo ra một giá trị độc nhất cho phần đuôi được thêm vào của mỗi đoạn dữ liệu khiến chúng không bị khai thác. Điều này khiến hash hoàn toàn khoogn thể bị đảo ngược.
* Có thể dễ dàng hash một đầu vào cho sẵn, Nhưng không thể giải mã được đầu vào bị hash đó . Điều này có nghĩa nếu 1 client giữ đoạn dữ liệu chính xác, nó có thể giải mã hash và so sánh các giá trị của chúng để xác thực rằng chúng có sở hữu dữ liệu chuẩn hay không.
* SSH sử dụng hash để đảm bảo tính xác thực của các gói tin. Nó được thực hiện bằng cách sử dụng HMACs(Hash-based Message Authentication Codes). Điều này đảm bảo các lệnh mà máy đầu xa nhận được sẽ không bị giả mạo theo bất cứ cách nào
* Trong quá trình lựa chọn thuật toán mã hóa symmetric, một thuật toán xác định gói tin cũng sẽ được lựa chọn theo đúng cách lựa chọn symmetric.
* Mỗi gói tin truyền đi phải chứa đựng 1 MAC được tính toán dựa trên symmetric kry , packet sequence number và nội dung gói tin.
### 4) Cách kết hợp cả 3 công nghệ trên cả SSH
* SSH sử dụng mô hìn Client-Server đê cho phép các thực giữa 2 máy đầu xa và mã hóa dữ liệu truyền qua chúng.
* SSH hoạt động mặc định port `22` của giao thức tcp(có thể thay đổi được)
* Host (server) lắng nghe trên port `22` chờ kết nối đến.
* Host sẽ thiết lập kết nối bảo mật bằng việc xác thực client và mở một môi trường shell tương ứng khi việc xác thực thành công.
* CLient sẽ bắt đầu kết nối đến SSH bằng việc bắt tay 3 bước Tcp với host, đảm bảo kết nối bảo mật symmetric, xác thực ẩn danh trong server đúng với các bản ghi trước ( thường được lưu trữ trong file RSA), và trình diện đúng user được ủy quyền trên host để xác thực phiên kết nối.

Khi client cố kết nối tới server qua TCP, server sẽ trình ra các kỹ thuật mã hóa va những phiên bản liên quan đến nó hỗ trợ. Nếu client cũng có protocol tương ứng và phiên bản đúng như vậy, một thỏa thuận sẽ được đặt ra và kết nối bắt đầu tiếp nhận protocol. Server cũng sử dụng một symmeric 