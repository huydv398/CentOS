## Cách sử dụng Vim trong CentOS
1. Di chuyển nhanh đến chế độ đầu dòng cuối dòng

`$` -- Nhảy về Cuối dòng hiện tại

`0` -- Nhảy về Đầu dòng hiện tại
2. Di chuyển về đầu hoặc cuối file, sử dụng phím G hoặc gg

`gg` -- Nhảy lên Đầu file

`G` -- Nhảy xuống cuối file

`10G` -- Nhảy tới dòng 10 của file

`CTRL` + `G` -- Hiển thị thông tin dòng hiện tại, Tổng số dòng,...

3. Highlight

Khi cần bôi đen( highlight) một đoạn hoặc một dòng, ta dùng phím `v` và `V`
|PHÍM|CHỨC NĂNG|
|-|-|
|`v` | High một vùng text được chọn bằng phím di chuyển hoặc H/J/K/L|
|`V`|Chọn nhanh dòng hiện tại|
|`vap`|Chọn đoạn văn bản hiện tại|
|`ggVG`|Bôi đen toàn bộ file|

4. Tìm kiếm nội dung
* `/ *Nội-dung* ` : Tìm trong file từ vị trí con trỏ xuống dưới
* `? *Nội-dung*` : Tìm trong file từ vị trí con trỏ trở lên
* `n` : tiếp tục tìm kiếm với nội dung hiện tại

5. Chỉnh sửa nội dung
|-|-|
|`i`|Insert mode|bật chế độ chèn text|
|`R`|Replace mode|bật chế độ thay thế|
|`x`||xóa một ký tự ở vị trí con trỏ|
6. Các lệnh lưu và thoát
|Câu lệnh|Chức năng |
|-|-|
|:q|Thoát khỏi VIM|
|:q!|Bắt buộc thoát không lưu|
|:w|Lưu file|
|:w!|Bắt buộc ghi file (ghi đè)|
|:wq!|Lưu file rồi thoát ra|