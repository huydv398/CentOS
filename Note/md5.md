* Khi thực hiện `rsync` sử dụng giao thức cập nhật từ, chỉ chuyển sự khác biệt giữa các tệp hoặc nội dung thư mục đến đích. Nhưng khi nào biết đó là sự khác biệt? 

* Thực ra Linux có một cách để kiểm tra sự khác biệt đó là mã md5. 
* Mỗi file đều có gắn một mã md5 khi tạo hoặc có sự thay đổi.
* **Quan trọng** : md5 chỉ xác minh hoạt động với nội dung file chứ không phải tên file.
```
[root@server1 ~]# echo mot hai ba bon nam > Num.txt
[root@server1 ~]# echo 123456789 > num.txt

[root@server1 ~]# md5sum Num.txt  > md5.txt
[root@server1 ~]# md5sum num.txt  >> md5.txt

[root@server1 ~]# cat md5.txt
e91db47ae735fa20d793b433452c23e8  Num.txt
b2cfa4183267af678ea06c7407d4d6d8  num.txt

```
* Trên là mã md5 của 2 file mới tạo. Và mã này không trùng nhau.

* Thay đổi file

![Imgur](https://i.imgur.com/YGGnv44.png)

Khi check mã md5 cũ, do có sự thay đổi nên `FAILED`

* Update md5 và check lại:
![Imgur](https://i.imgur.com/k6jWyRD.png)
![Imgur](https://i.imgur.com/4V1EPnz.png)
* Sau khi check lại mã md5 hiện tại là mới nhất
![Imgur](https://i.imgur.com/s6mlD7V.png)

#### Trên là bài note md5 và cách check md5 khi file có sự đổi 