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
## Mount
* Cấu trúc lệnh:`  # mount [option] [device_name] [mount_point]`

    * option: 
        * `-v`: chế độ chi tiết, cung cấp thông tin về những gì mount định thực hiện
        * `-w`: mount hệ thống tệp tin với quyền đọc và ghi
        * `-r`: mount hệ thống tập tin chỉ có quyền đọc
        * `-t` : xác định lại hệ thống tập tin được mount. Những loại hợp lệ là `ext2`, `ext3`, `ext4`, `xfs`,`iso9660`
        * `-a`: mount tất cả các hệ thống tập tin được khai báo trong `fstab`
        * `o`: remount chỉ didhj việc mount tại 1 file system nào đó

### Thêm 1  CD/DVD Drive kết nối vào Server 
* Thêm CD/DVD. Thực hiện lệnh kiểm tra `lsblk`:
```
[root@localhost ~]# lsblk
NAME      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
...
sr0        11:0    1  3.8G  0 rom
```
* Tạo thư mục để mount CD:
` mkdir /mnt/cd`
* Thực hiện lệnh mount:
`mount /dev/sr0 /mnt/cd`
* Kiểm tra lại mount point bằng câu lệnh `df -h`
```

[root@localhost ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        476M     0  476M   0% /dev
tmpfs           487M     0  487M   0% /dev/shm
tmpfs           487M  7.7M  479M   2% /run
tmpfs           487M     0  487M   0% /sys/fs/cgroup
/dev/sda3        19G  1.9G   17G  11% /
/dev/sda1       509M  152M  357M  30% /boot
tmpfs            98M     0   98M   0% /run/user/0
/dev/sr0        3.9G  3.9G     0 100% /mnt/cd
```
* Nếu là tạm thời thì đã có thể sử dụng nhưng nếu cần dùng vĩnh viễn thì cần thêm `/dev/sr0 /mnt/cd auto 0 0` vào file `/etc/fstab` :

` echo /dev/sr0 /mnt/cd [TYPE] auto 0 0 >> /etc/fstab`
>## Chú ý:
cấu trúc câu lệnh thêm vào tệp phải đúng nếu không có thể gây ra lỗi boot

* Mở thư mục kiểm tra:
```
[root@localhost ~]# ls /mnt/cd
autorun.inf  boot  bootmgr  bootmgr.efi  efi  setup.exe  sources  support
``` 