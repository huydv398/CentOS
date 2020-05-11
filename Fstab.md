# Fstab
## Fstab là gì?
Fstab là bảng hệ thống tệp hệ điều hành của bạn. Tệp fstab thường liệt kê tất cả các phân vùng đĩa có sẵn và các loại hệ thống tệp và nguồn dữ liệu khác không nhất thiết phải dựa trên đĩa và cho biết cách chúng được khởi tạo hoặc tích hợp vào cấu trúc hệ thống tẹp lớn hơn.

Tệp fstab được đọc bởi lệnh `mount`, tự động sảy ra khi khởi động để xác định cấu trúc tệp tổng thể và sau đó khi người dùng thực thi lệnh `mount` để sửa đổi cấu trúc đó. Quản trị viên hệ thống có nhiệm vụ tạo và duy trì đúng tệp fstab.

Mặc dù fstab vẫn được sử dụng cho cấu hình hệ thống cơ bản, nhưng đối với các mục đính sử dụng khác, nó đã được thay thế bằng các cơ chế lắp tự động
## blkid
Câu lệnh dùng để in ra các thuộc tính sau:
* Vị trí các thiết bị trên máy.
* **UUID**: Là một chuẩn định danh dùng trong các kiến trúc phần mềm, được quy định bởi tổ chức OSF.
* Type: loại file system 
```
[root@localhost ~]# blkid
/dev/sda1: UUID="5256a78f-d5b2-4d08-901b-f952f6ae2f1b" TYPE="xfs"
/dev/sda2: UUID="4Ykd6a-J5gA-0ko9-xj1d-OsOc-SBpw-DJoFME" TYPE="LVM2_member"
/dev/sr0: UUID="2019-09-11-18-50-31-00" LABEL="CentOS 7 x86_64" TYPE="iso9660" PTTYPE="dos"
/dev/mapper/centos-root: UUID="a7634862-b5a0-4811-9998-e4341cca6032" TYPE="xfs"
/dev/mapper/centos-swap: UUID="78b7603a-fab4-4bb7-8d6c-7983edf77520" TYPE="swap"
```
## Mô tả sơ lược về fstab
Đây là cấu trúc của file `fstab`:
```
1             2          3       4        5     6
/dev/sr0     /demo      iso9660 auto      0     0

```
|Số thứ tự|Ý nghĩa|
|-|-|
|1|Thiết bị cần `mount` là ổ đĩa hoặc thiết bị|
|2|Nơi `mount` mặc định|
|3|Loại file system.Ví dụ như file hệ thống là `xfs`, `swap`,`iso9660`|
|4|Các tùy chọn `mount`.`auto - noautoo` thiết bị sẽ tự động `mount` khi khởi động hoặc không.|
|5|`dump` môt tiện ích backup file|
|6|quy định thứ tự để kiểm tra|

Một số các option khi sử dụng mount(cột 4) :
|option|ý nghĩa|
|-|-|
|user - nouser |Tùy chọn user cho phép những user bình thường có thể muont thiết bị,va ngược lại chỉ cho phép root mới có quyền muont|
|exec - noexec|Cho phép bạn thực thi các file thực thi tồn tại trên partition đó. Tùy chọn nouser là mặc định|
|rw| mount partition ở chế độ read-write, đôi khi bạn không thể ghi vào đĩa mềm mà không hiểu tại sao|
|ro|mount partition ở chế độ read-only, đôi khi không thể ghi vào đĩa mềm|
|sync - async |Tùy chọn cho việc đọc và ghi file system. sync là tất cả được làm đồng thời với nhau, tùy chọn này thường được áp dụng cho đĩa mềm. |
|defaults |sử dụng các tùy chọn mặc định đó là rw, suid, dev, exec, auto,nouser và async các tùy chọn được cách nhau bằng dấu phẩy |