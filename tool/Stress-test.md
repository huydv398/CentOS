# Hướng dẫn cài đặt sử dụng Stress test 
## Stress test để làm gì?
**Stress** là một câu lệnh được sử dụng để áp dụng một mức tải cho hệ thống để kiểm tra khả năng chịu tải của nó trong thực tế. Nó sẽ tạo ra các process sử dụng rất nhiều tài nguyên của hệ thống như CPU,RAM hay Disk để ta có thể kiểm tra được khả năng và ngưỡng mà hệ thống của ta hoạt động tốt nhất. Nó cũng rất hữu ích để tái hiện lại trạng thái tải cao gay lỗi hệ thống để ta có thể tìm cách xử lý.
### Cài đặt stress test 
Để sử dụng được stress test bạn cần có kho `Epel` tải cài đặt kho ta có :
* `yum install -y epel`
Nếu đã có kho `epel` ta cài đặt `Stress` bằng câu lệnh:
* `yum install -y stress`
## Sử dụng
Bạn có thể sử dụng **Stress** với các thông số CPU, Memory, io, HDD, hoặc `stress` cùng lúc nhiều thông số.
Dưới đây tôi sẽ ví dụ một số trường hợp cụ thể để bạn có thể biết cách sử dụng nó
### Test CPU
Để test CPU ta dùng 
* `stress -c [option] --timeout [thời gian được tính bằng giây]`
    * `option`là số tiến trình được đẩy vào cpu cùng 1 lúc
```
stress -c 1
[root@localhost ~]# top
Tasks: 112 total,   3 running, 109 sleeping,   0 stopped,   0 zombie
%Cpu(s): 99.7 us,  0.3 sy,  0.0 ni,  0.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st

   PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
  9085 root      20   0    7312     96      0 R 99.3  0.0   0:24.12 stress

stress -c 3

%Cpu(s): 99.3 us,  0.7 sy,  0.0 ni,  0.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st

   PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
  9091 root      20   0    7312     96      0 R 33.6  0.0   0:01.34 stress
  9090 root      20   0    7312     96      0 R 33.2  0.0   0:01.34 stress
  9092 root      20   0    7312     96      0 R 32.9  0.0   0:01.33 stress
```
Đọc để biết chi tiết hơn với dòng lệnh của `top`: https://github.com/huydv398/CentOS/blob/master/Tutorial/processes.md#top

## Test Memory
Để đẩy tải vào Ram để test ta dùng option `-n` hoặc `--vm-byte`
* Command : stress -m 1 --vm-bytes 500M
## Test CPU
* Command : stress -i 1

 Câu lệnh tiến hành ghi một file có dung lượng 1G vào disk. Bạn có thể chỉ ra dung lượng của nó bằng cách thêm option --hdd-bytes. Ví dụ muốn ghi 1 file 2G
 * Command :` stress -d 1 2G --timeout 1m`

 