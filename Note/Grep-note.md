# Grep là lệnh tìm văn bản trên Linux.

* Command: 
    * `grep [option] "[string]" [Source_file] `
    
    Hoặc

    * ` cat [file] | grep [option] [string]`

* Tìm một chuỗi trong file.
    * Command: `grep "[string]" [file]`

* Tìm chuỗi trên nhiều file.
    * Command: `grep '[string]' [file1] [file2]`
        * Tìm kiếm `string` bên trong file1 và file2

* Tìm kiếm không phân biệt chữ hoa thường
    * Command: ` grep '[string]' -i [file]`

* Tìm kiếm ngược.
    * Command : `grep -v '[string]' file`.
    * Hiển thị các dòng không có ký tự `string` trong file

* Hiển thị số dòng, số lượng, giới hạn số dòng đầu ra.
    * Command: `grep -n -c -m(x) '[string]' file`
        * `-n` : Hiển thị số thứ tự của dòng và dòng chứa từ cần tìm
        * `-c` : Đếm số dòng khớp với từ cần tìm
        * `-m(x)` : x là chỉ số cần giới hạn số lượng dòng mà khớp với `string` cần tìm.
    
* Tìm kiếm nhiều chuỗi
    * `grep -e 'word1' -e 'word2' [file]`
    
    Hoặc

    * `grep 'word1\|word2' [file]`

    Hoặc

    * `egrep 'word1|word2' [file]`

* Tìm kiếm tên tệp
    * Command: `grep [-l/-L/-h/-H] 'string' [folder]`
    
    * `-l` : Để có được các tập tin phù hợp với tìm kiếm. Sẽ in ra kết quả là những tên file có chứa từ cần tìm
    * `-L` : Để có được các tệp không phù hợp với tìm kiếm
    Sẽ in ra kết quả là những tên file không chứa ký tự `string`
    
    * `-h` : Chỉ hiện thị các dòng có trong các file. Cho kết quả những dòng chứa `string` mà không kèm tên file.
    * `-H` : Hiện thị tên file + mỗi dòng khớp trả về. Kết quả sẽ bao gồm tên file và dòng chứa `string`

* Hiển thị thêm dòng trước, sau, xung quanh dòng chứa kết quả cần tìm.
    * Command: `grep -[A, B hoặc C] (n) ['string'] [file]`
        * `A` :Hiển thị dòng khớp với ký tự được tìm và dòng dưới nó.
        * `B` : Hiển thị dòng khớp với ký tự được tìm và dòng trên của nó
        * `C` : Hiện thị dòng xung quanh dòng khớp với kí tự cần tìm.Vidu C2 thì hiện thị 2 dòng trên và 2 dòng dưới của dòng được khớp.
        * `n` : Là số tự nhiên chỉ định xem hiển thị trước, sau hay xunh quanh bao nhiêu dòng

* Tìm chính xác với `-w` 
    * Command: `grep -w '[string]' file
    * Kết quả sẽ tìm chính xác chỉ những dòng có chứa từ `string`
* Tìm tất cả các file trên thư mục con và thư mục cha với tùy chọn `-R`

* Đọc file loại bỏ Comment, dòng trống:

    * Command: `egrep -v "^#|^*#|^$" file`
        * `-v` : Tìm các dòng không chứa các ký tự...
        * `^#` : Những dòng bắt đầu bằng #
        * `^*#` : Những dòng bắt đầu bằng # kể cả khoảng chắn trước .
        * `^$` : Những dòng trống
    