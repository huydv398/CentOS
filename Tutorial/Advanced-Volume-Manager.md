# Quản lý khối nâng cao
Tạo bố cục LVM
```
[root@sv ~]# pvcreate /dev/sdb1
  Physical volume "/dev/sdb1" successfully created.
[root@sv ~]# vgcreate vgdb /dev/sdb1
  Volume group "vgdb" successfully created
[root@sv ~]# lvcreate -l 100%FREE -n lvol1 vgdb
  Logical volume "lvol1" created.
[root@sv ~]# pvs
  PV         VG     Fmt  Attr PSize   PFree
  /dev/sda2  centos lvm2 a--  <19.00g    0
  /dev/sdb1  vgdb   lvm2 a--  <10.00g    0
[root@sv ~]# vgs
  VG     #PV #LV #SN Attr   VSize   VFree
  centos   1   2   0 wz--n- <19.00g    0
  vgdb     1   1   0 wz--n- <10.00g    0
[root@sv ~]# lvs
  LV    VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root  centos -wi-ao---- <17.00g                           
  swap  centos -wi-ao----   2.00g                           
  lvol1 vgdb   -wi-a----- <10.00g                           
```
Tạo một `ext3` hệ thống tệp trên ổ đĩa logic và gắn kết phân vùng trong một thư mục `/db`
```
[root@localhost ~]#  mkfs.ext4 /dev/vg/db
[root@localhost ~]# mkdir /db
[root@localhost ~]#  mount /dev/vg/db /db/
```

Cài đặt cơ sở dữ liệu mysql trên hệ thống tệp tin mới
```
[root@localhost ~]# rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
[root@localhost ~]# yum install -y mysql-server
