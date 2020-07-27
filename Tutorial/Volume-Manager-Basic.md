# Logical Volume Manager - LVM
1. [Thông tin cơ bản về quản lý khối](#infolvm)
2. [Các khối cơ bản của Logical Volume Manager ](#volbasic)
3. [Các thành phần trong LVM](#thanhphan)
4. [Ưu điểm và nhược điểm của LVM](#uunhuoc)
5. [Các lệnh trong LVM](#command)
6. [Tạo mới Partition ](#linear)
7. [Các thao tác trên LVM](#thaotac)
8. [Thay đổi dung lượng](#change)
9. [Xóa Logical Volume, Volume group,physical Volume](#del)
<a name="infolvm"></a>
## Thông tin cơ bản về quản lý khối
* **Logical Volume Manger (LVM)** là phương pháp cho phép ấn định không gian đĩa cứng thành những **Logiccal Volume** khiến cho việc thay đổi kích thước trở nên dễ dàng hơn so với **partition**.
* Với ký thuật **LVM**, có thể thay đổi kích thước phân vùng mà không cần phải sửa lại table của OS. Điều này rất hữu ích với trường hợp đã sử dụng hết bộ nhớ trống của partition và muốn mở rộng dung lượng của nó.
* Vai trò của **LVM**: **LVM** là kỹ thuật quản lý việc thay đổi kích thước lưu trữ của ổ cứng:
    * Không để hệ thống bị gián đoạn hoạt động
    * Không làm hỏng dịch vụ
    * Có thể kết hợp cơ chế **hot Swapping**(Phương pháp thay thế nóng các thành phần bên trong máy tính)
* Mục đích:
    * Tạo 1 hoặc nhiều phân vùng logic hoặc phân vùng với toàn bộ đĩa cứng, Cho phép thay đổi kích thước khối
    * Quản lý trang đĩa cứng lớn( Large hard disk Farms ) bằng cách cho phép thêm và thay thế đĩa mà không bị ngừng hoạt động hoặc gián đoạn dịch vụ, kết hợp với trao đổi nóng(hot swap)
    * Trên hệ thống nhỏ (như laptop,pc), thay vì phải ước tính thời gian cài đặt, phân vùng có thể cần lớn đến mức nào, LVM cho phép hệ thống tệp dễ dàng thay đổi kích thước khi cần .
    * Thực hiện sao lưu nhất quán bằng cách tạo ra snapshot nhanh các khối cách hợp lý.
    * Mã hóa nhiều phân vùng vật lý bằng một mật khẩu
<a name="volbasic"></a>
## Các khối cơ bản của **LVM**(Logical Vulome Manager ) bao gồm:
* Physical Volumes: là những đĩa cứng vật lý hoặc phân vùng trên nó
* Volume groups: là một nhóm bao gồm các *Physical Volumes*.Có thể xem Volume group như một " ổ đĩa ảo".
* Logical volumes: có thể xem như là các "phân vùng ảo" bạn có thể thêm vào gỡ bỏ và thay đổi kích thước một cách nhanh chóng.*(/dev/sda1, /dev/sdb1, /dev/sdc1)*
<a name="thanhphan"></a>
## Các thành phần trong LVM
### 1. Hard drives
* Là các thiết bị lưu trữ dữ liệu có dạng: `/dev/sda`,`/dev/sdc2`,...
### 2. Partition 
* Là các phân vùng của **Hard Drives**, mỗi **Hard** có 4 **Partition** trong đó **Partition** gồm 2 loại là **primary partition** và **extended partition** :
* **Primary Partition** :
    * Là phân vùng chính, có thể `Boot`
    * Mỗi đĩa cứng có thể có tối đa 4 phân vùng này
* **Extended Partition** :
    * Là phân vùng mổ rộng chứa dữ liệu, trong nó có thể tạo các **logical partition** 
### 3. Physical Volume(PV)
* Là 1 tên gọi khác của **partition** trong kỹ thuật **LVM**, nó là những thành phần cơ bản được sử dụng bởi **LVM** 
* Một **Physical Volume** không thể mở rộng ra ngoài phạm vi 1 ổ đĩa
* có thể kết hợp nhiều **physical Volume** thành một **Volume Group**.
### 4. Volume group
* Nhiều **Physical Volume** trên những ở đĩa khác nhau kết hợp lại thành 1 **Volume Group**
* **Volume Group** được dùng để tạo ra các **Logical Volume**, trong đó người dùng có thể tạo, thay đổi kích thước, Gỡ bỏ và sử dụng chúng
### 5.Logical Volume 
* **Volume Group** được chia nhỏ thành các **Logical Volume**,Mỗi **Logical Volume** có ý nghĩa tương tự 1 Partition. Nó được dùng cho các mount point và được format với những định dạng khác nhau như `ext2`, `ext3`, `ext4`, `xfs`, ...
* Khi dung lượng của **Physical Volume** được sử dụng hết ta có thể thêm ổ đĩa mới bổ sung cho **Volume Group** và do đó tăng được dung lượng của **Logical Volume**

VD: Có thể tạo ra 4 ổ đĩa mỗi ổ `5GB`, kết hợp thành 1 **Volume Group** `20GB`, có thể tạo ra 2 **Logical Volume** mỗi cái `10GB`
<a name="uunhuoc"></a>
## Ưu nhược điểm của LVM
### Ưu điểm
* Có thể gom nhiều đĩa cứng vật lý thành 1 đỉa ảo dung lượng lớn
* Có thể tạo ra các vùng dung lượng lớn tùy ý 
* Có thể thay đổi các vùng dung lượng đó dễ dàng, linh hoạt
### Nhược điểm
* Các bước thiết lập phức tạp, khó khăn hơn
* Càng gắn nhiều đĩa cứng và thiết lập càng nhiều **LVM** thì hệ thống thì hệ thống khởi động càng lâu
* Khả năng mất dữ liệu khi 1 trong các đĩa cứng vật lý bị hỏng
* Tiêu hao nhiều năng lượng không cần thiết
<a name="command"></a>
## Các lệnh trong LVM
* **Physical Volume**:
    * `pvcreate`:tạo **Physical Volume**
    * `pvdisplay`, `pvs`: xem **Physical Volume** đã tạo
    * `pvremove`: xóa **Phyysical Volume**
    * `pvchange`: Thay đổi thuộc tính của Physical volume
* **Volume Group**:
    * `vgcreate`: Tạo **Volume Group**
    * `vsdisplay`,`vgs`: xem **Volume Group** đã tạo
    * `vgremove` : xóa **Volume Group**
    * `vgextend` : tăng dung lượng của **Volume Group**
    * `vgreduce`: giảm dung lượng của **Volume Group**
* **Logical Volume**:
    * `lvcreate`: Tạo **Logical Volume**
    * `lvdisplay`, `lvs`: xem **Logical Volume** đã tạo
    * `lvremove`: xóa **Logical Volume**
    * `lvextend`: tăng dung lượng **Logical Volume**
    * `lvreduce`: giảm dung lượng **Logical Volume**

<a name="linear"></a>
## Các bước tạo Linear Volume
* Bổ sung thêm 2 Ổ cứng dung `lượng 20Gb`
* Kiểm tra các **Hard Drives** có trên hệ thống:
`# lsblk`
Hoặc

`#fdisk -l` 
```
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0   20G  0 disk
├─sda1            8:1    0    1G  0 part /boot
└─sda2            8:2    0   19G  0 part
  ├─centos-root 253:0    0   17G  0 lvm  /
  └─centos-swap 253:1    0    2G  0 lvm  [SWAP]
sdb               8:16   0   20G  0 disk
sdc               8:32   0   20G  0 disk
sr0              11:0    1  4.4G  0 rom
```
* Tạo **Partition**"
    * Từ các Hard Disk trên hệ thống, tạo ra các Partition.
    * Dùng lệnh `fdisk`(có thể dùng `parted`)
    ```
    [root@sv ~]# fdisk /dev/sdc
    Welcome to fdisk (util-linux 2.23.2).

    Changes will remain in memory only, until you decide to write them.
    Be careful before using the write command.

    Device does not contain a recognized partition table
    Building a new DOS disklabel with disk identifier 0xd13bfd56.

    Command (m for help): m
    ```
    * `n`: tạo Partition mới
    ```
    Command (m for help): n
    Partition type:
    p   primary (0 primary, 0 extended, 4 free)
    e   extended
    Select (default p): p
    ```
    * `p`: tạo Partition `Primary`
    ```
    Command (m for help): n
    Partition type:
    p   primary (0 primary, 0 extended, 4 free)
    e   extended
    Select (default p): p
    ```
    * `1` Tạo Partition thứ nhất
    ```
    Partition number (1-4, default 1): 1
    ```
    * `First sector (2048-41943039, default 2048):    `
    để **default**
    * `Last sector, +sectors or +size{K,M,G} (2048-41943039, default 41943039):+10G `
    +10G để tạo phân vùng có size là 10GB
    * `t` : tùy chọn thay đổi định dạng Partition 
    * `8e` : thay đổi về định dạng **LVM**
    * `w` : Lưu lại và thoát
* Kiểm tra lại:
```
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0   20G  0 disk
├─sda1            8:1    0    1G  0 part /boot
└─sda2            8:2    0   19G  0 part
  ├─centos-root 253:0    0   17G  0 lvm  /
  └─centos-swap 253:1    0    2G  0 lvm  [SWAP]
sdb               8:16   0   20G  0 disk
├─sdb1            8:17   0   10G  0 part
└─sdb2            8:18   0   10G  0 part
sdc               8:32   0   20G  0 disk
└─sdc1            8:33   0   10G  0 part
sr0              11:0    1  4.4G  0 rom
```
<a name="thaotac"></a>
## Thao tác trên LVM 
Liệt kê các phân vùng ổ cứng trong hệ thống:`cat /proc/partition `, `fdisk -l` hoặc ` ls -la /dev/sd*`
### Mục tiêu
* Sử dụng LVM để phân chia sdb và sdc thành 2 logical volume có tên `Data` và `Backups`
* `Muont` logic volumes
* Giảm kích thước `Backups`
* Tăng kích thước `Data`

### Tạo Physical Volume
Tạo 2 Physical volume từ 2 ổ `sdb` và `sdc`: 
```
[root@sv ~]# pvcreate /dev/sdb /dev/sdc
  Physical volume "/dev/sdb" successfully created.
  Physical volume "/dev/sdc" successfully created.
```
### Hiển thị thông tin về Physical Volumes
* command : `# pvdisplay`
### Tạo Volume Group(VG)
Lệnh kiểm tra những VG hiện có : `#vgs`
```
[root@sv ~]# vgs
  VG     #PV #LV #SN Attr   VSize   VFree
  centos   1   2   0 wz--n- <19.00g    0
```

Tạo 1 VG có tên VG1 từ 2 Phyysical Volume vừa tạo ở trên
```
[root@sv ~]# vgcreate VG1 /dev/sdb /dev/sdc
  Volume group "VG1" successfully created
```
Xem lại thông tin về VG1 ta vừa tạo ra:
```
  --- Volume group ---
  VG Name               VG1
  System ID
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
```
### Tạo Logical Volume
Lệnh kiểm tra xem có những LV nào :`#lvs`
```
[root@sv ~]# lvs
  LV   VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root centos -wi-ao---- <17.00g                           
  swap centos -wi-ao----   2.00g 
 ```

Ta tạo ra 2 Logical Volume là `Data` và `Backups` Như đã hữa ở trên.
* command : lvc -n [tên_Logical_Volume] -L [dung_lượng] [tên_Volume_Group]
    * Trong đó:
        * -n :(name) Đặt tên
        * -L : Sử dụng kích thước cố định
        * -l : Sử dụng % của không gian còn lại trong Volume

```
[root@sv ~]# lvcreate -n Date -L 900m VG1
  Logical volume "Date" created.
[root@sv ~]# lvcreate -n Backups -l 100%FREE VG1
  Logical volume "Backups" created.
```
Kiểm tra thông tin các Logical vừa tạo: `# lvdisplay`
```
 --- Logical volume ---
  LV Path                /dev/VG1/Date
  LV Name                Date
...
--- Logical volume ---
  LV Path                /dev/VG1/Backups
  LV Name                Backups
  VG Name                VG1
...
```
## Định dạng Logical Vulume
`Ext4` : Cũng giống như `Ext3`, lưu trữ được những ưu điểm và tính tương thích ngược với phiên bản trước đó. Như vậy, chúng ta có thể dễ dàng kết hợp các phân vùng định dạng Ext2, Ext3 và Ext4 trong cùng 1 ổ đĩa trong CentOS để tăng hiệu suất hoạt động. Trên thực tế, `Ext4` có thể giảm bớt hiện tưởng phân mảnh dữ liệu ổ cứng, hỗ trợ các file và phân vùng có dung lượng lớn.. Thích hợp với SSD so với `Ext3`, tốc độ hoạt động nhanh hơn so với 2 phiên bản Ext trước đó, cũng khá phù hợp để hoạt động trên server, nhưng lại không bằng `Ext3`.

Ta định dạng Logical Volume ở dạng `Ext4`:
* [root@sv ~]# mkfs.ext4 /dev/VG1/Backups
* [root@sv ~]# mkfs.ext4 /dev/VG1/Date
## Mount Logical Volume
Ta cần Mount Logical Volume để kiểm tra việc tạo ra thành công:
```
[root@sv ~]# mkdir /Data
[root@sv ~]# mount /dev/VG1/Data /Data/
[root@sv ~]# mkdir /Backups
[root@sv ~]# mount /dev/VG1/Backups /Backups/
```
<a name="change"></a>
## Thay đổi kích thước Logical Volume
### Giảm kích thước LV
Trước khi bắt đầu, Cần sao lưu dữ liệu vì vậy sẽ được trách sự cố sảy ra. Cần thực hiện cẩn thận 5 bước dưới đây:
* Ngắt kết nối FileSystem.
* Kiểm tra FileSystem sau khi ngắt kết nối.
* Giảm FileSystem
* Giảm kích thước Logical Volume hơn kích thước hiện tại.
* `mount` lại FileSystem và kiểm tra kích thước của nó.

Ta sẽ giảm kích thước của LV `Backups` từ 2,2G xuống 2G mà không làm mất dữ liệu.
### Kiểm tra dung lượng của Logical Volume và kiểm tra FileSystem trước khi thực hiện giảm
* `# lvdisplay VG1`
* `# df -TH`
```
/dev/mapper/VG1-Data    ext4      809M  1.7M  749M   1% /Data
/dev/mapper/VG1-Backups ext4      2.3G  7.0M  2.2G   1% /Backups
```
### Unmount Logical Volume Backups và kiểm tra trạng thái mount
```
[root@sv ~]# umount /Backups/
[root@sv ~]# df -TH
Filesystem              Type      Size  Used Avail Use% Mounted on
devtmpfs                devtmpfs  498M     0  498M   0% /dev
tmpfs                   tmpfs     510M     0  510M   0% /dev/shm
tmpfs                   tmpfs     510M  8.1M  502M   2% /run
tmpfs                   tmpfs     510M     0  510M   0% /sys/fs/cgroup
/dev/mapper/centos-root xfs        19G  1.4G   17G   8% /
/dev/sda1               xfs       1.1G  143M  921M  14% /boot
tmpfs                   tmpfs     102M     0  102M   0% /run/user/0
tmpfs                   tmpfs     102M     0  102M   0% /run/user/1001
/dev/mapper/VG1-Data    ext4      809M  1.7M  749M   1% /Data
[root@sv ~]#
```
VG1-Backups đã không tồn tại
### Kiểm tra lỗi
* `# e2fsck -f /dev/VG0/Backups`
```
[root@sv ~]# e2fsck -f /dev/VG1/Backups
e2fsck 1.42.9 (28-Dec-2013)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/VG1/Backups: 11/145152 files (0.0% non-contiguous), 27202/579584 blocks
```
### Giảm kích thước Logical Volume `Backups`
* `# lvreduce -L [kích_thước] /dev/<tên_group>/<tên_logical>`
    * `-L` :Chọn kích thước chuẩn
```
[root@sv ~]# lvreduce -L 500M /dev/VG1/Backups
  WARNING: Reducing active logical volume to 500.00 MiB.
  THIS MAY DESTROY YOUR DATA (filesystem etc.)
Do you really want to reduce VG1/Backups? [y/n]: y
  Size of logical volume VG1/Backups changed from 2.21 GiB (566 extents) to 500.00 MiB (125 extents).
  Logical volume VG1/Backups successfully resized.
```

* Sau đó, ta xác nhận thay đổi bằng cách sử dụng lệnh `resize2fs`
    * ` # resize2fs /dev/<tên_group>/<tên_volume>`

* Định dạng lại Volume về Ext4:
    * `[root@sv ~]# mkfs.ext4 /dev/VG1/Backups`
* Kiểm tra lỗi:
    * `[root@sv ~]# e2fsck -f /dev/VG1/Backups`
* Mount lại file và kiểm tra kích thước của nó
```
    [root@sv ~]# mount /dev/VG1/Backups /Backups/
    [root@sv ~]# df -TH
    Filesystem              Type      Size  Used Avail Use% Mounted on
    devtmpfs                devtmpfs  498M     0  498M   0% /dev
    tmpfs                   tmpfs     510M     0  510M   0% /dev/shm
    tmpfs                   tmpfs     510M  8.1M  502M   2% /run
    tmpfs                   tmpfs     510M     0  510M   0% /sys/fs/cgroup
    /dev/mapper/centos-root xfs        19G  1.4G   17G   8% /
    /dev/sda1               xfs       1.1G  143M  921M  14% /boot
    tmpfs                   tmpfs     102M     0  102M   0% /run/user/0
    /dev/mapper/VG1-Backups ext4      500M  2.4M  467M   1% /Backups
    /dev/mapper/VG1-Data    ext4      809M  1.7M  749M   1% /Data
```
### Tăng kích thước LV
#### Kiểm tra kích thước và umount LV `Data`
```
[root@sv ~]# umount /Data
```
#### Mở rộng kích thước LV
Ta sẽ tăng kích thước Data lên ~ 1G(dung lượng còn lại của tổng `sdb` và `sdc`) 
```
[root@sv ~]# lvextend -l +100%FREE /dev/VG1/Data
  Size of logical volume VG1/Data changed from 800.00 MiB (200 extents) to 2.50 GiB (641 extents).
  Logical volume VG1/Data successfully resized.
```
Kiểm tra lỗi và xác nhận thay đổi:
```
[root@sv ~]# e2fsck -f /dev/VG1/Data
[root@sv ~]# lvextend -l +100%FREE /dev/VG1/Data
```

Kiểm tra lại dung lượng:
```
[root@sv ~]# lvdisplay VG1
```
Muont lại LV `Data` và kiểm tra kích thước:
```
[root@localhost ~]# mount /dev/VG0/Data /Data/
```
<a name="del"></a>
## Xóa 1 logical Volume và 1 Group Volume
### Xóa 1 Logical Volume
```
lvremove /dev/<tên_group>/<tên_Logical>
```
- Xóa 1 Logical Volume :
    - Kiểm tra những LV hiện có:
    ```
    [root@sv ~]# lvs
  LV      VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  Backups VG1    -wi-ao---- 500.00m                         
  Data    VG1    -wi-a-----   2.50g                         
  root    centos -wi-ao---- <17.00g                         
  swap    centos -wi-ao----   2.00g                         
    ```
    * Umount Data:`# umount /Data/`
    * Disable LV Data:`# lvchange -an /dev/VG0/Data`
    * Xóa LV Data:`# lvremove /dev/VG0/Data`
    * Kiểm tra lại danh sách LV :`# lvs`
## Xóa 1 Group Volume
`# vgremove /dev/<tên_Group_Volume>`
* Kiểm tra các danh sách GV:`# vgs`
* Disable Volume Group : `# vgchange -an /dev/VG1`
* Remove GV: `#vgremove /dev/VG1`
* Kiểm tra lại các VG: `# vgs`
## Xóa Physical Volume
`# pvremove /dev/sdb`

https://github.com/hoangdh/Su_dung_LVM