# Các lệnh cơ bản trong Linux  
## Mục lục  
[1. man page](#1)  
[2. Các lệnh làm việc với thư mục](#2)  
       [2.1 pwd](#2)  
       [2.2 cd](#3)  
       [2.3 ls](#4)  
       [2.4 mkdir](#5)  
       [2.5 rmdir](#6)  
[3. Các lệnh làm việc với tệp](#7)  
       [3.1 touch](#7)  
       [3.2 rm](#8)  
       [3.3 cp](#9)  
       [3.4 mv](#10)   
       [3.5 rename](#11) 


<a name="1"></a>
# Phần 1. man pages 

## 1.1 man $command  
Khi gặp một lệnh mà bạn không hiểu hoặc chưa biết cách sử dụng thì ta có thể gõ `man` đứng trước lệnh đó.  
```
man cat
CAT(1)                                                      User Commands                                                      CAT(1)

NAME
       cat - concatenate files and print on the standard output

SYNOPSIS
       cat [OPTION]... [FILE]...

DESCRIPTION
       Concatenate FILE(s), or standard input, to standard output.

       -A, --show-all
              equivalent to -vET

       -b, --number-nonblank
              number nonempty output lines, overrides -n

       -e     equivalent to -vE

       -E, --show-ends
              display $ at end of each line

       -n, --number
              number all output lines

       -s, --squeeze-blank
              suppress repeated empty output lines

       -t     equivalent to -vT

       -T, --show-tabs
              display TAB characters as ^I

       -u     (ignored)

       -v, --show-nonprinting
              use ^ and M- notation, except for LFD and TAB
```
Ta ấn `q` để thoát.  

## 1.2 man $configfile
Hầu hết các tập tin cấu hình đều có hướng dẫn riêng của nó  
```
man rsyslog.conf
RSYSLOG.CONF(5)                                      Linux System Administration                                      RSYSLOG.CONF(5)

NAME
       rsyslog.conf - rsyslogd(8) configuration file

DESCRIPTION
       The  rsyslog.conf  file  is  the main configuration file for the rsyslogd(8) which logs system messages on *nix systems.  This
       file specifies rules for logging.  For special features see the rsyslogd(8) manpage. Rsyslog.conf is backward-compatible  with
       sysklogd's syslog.conf file. So if you migrate from sysklogd you can rename it and it should work.

       Note  that this version of rsyslog ships with extensive documentation in html format.  This is provided in the ./doc subdirec‐
       tory and probably in a separate package if you installed rsyslog via a packaging system.  To use rsyslog's advanced  features,
       you need to look at the html documentation, because the man pages only cover basic aspects of operation.

MODULES
       Rsyslog  has  a  modular design. Consequently, there is a growing number of modules. See the html documentation for their full
       description.
...
```

## 1.3 man $daemon  
-> Hiển thị thông tin các chương trình chạy nền trên hệ thống  

## 1.4 man -k (apropos)
-> Hiển thị danh sách các manpage chứa chuỗi  
```
[root@centos7srv ~]# man -k syslog
logger (1)           - a shell command interface to the syslog(3) system log module
rsyslog.conf (5)     - rsyslogd(8) configuration file
rsyslogd (8)         - reliable and extended syslogd
```
## 1.5 whatis  
Để chỉ xem mô tả của một trang thủ công, sử dụng `whatis` theo sau bởi một chuỗi.  
```
[root@centos7srv ~]# whatis route
tc-route (8)         - route traffic control filter
```

## 1.6 whereis  
Vị trí của một `manpage` (tài liệu hướng dẫn sử dụng của program) có thể được hiển thị thông qua lệnh `whereis`  
Ngoài ra Lệnh `whereis` được sử dụng để xác định vị trí lưu trữ các binary file, source code, manual page của 1 chương trình (program) trên máy tính.

**Cú pháp cơ bản**  
```
whereis [option(s)] program_name(s)
```
Các tùy chọn:  
`-b` : tìm kiếm binary file  
`-m`: tìm kiếm man page  
`-s`: tìm kiếm source code

Khi không đi kèm tùy chọn, `whereis` trả về đường dẫn tuyệt đối của binary file, source code, man page (nếu chúng tồn tại) cho program_name (hay tên lệnh).  

```
[root@centos7srv ~]# whereis ls
ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz
```
```
[root@centos7srv ~]# whereis -b ls
ls: /usr/bin/ls
```
```
[root@centos7srv ~]# whereis -m ls
ls: /usr/share/man/man1/ls.1.gz
```
## 1.7 man sestions  
Ý nghĩa của những số nằm trong dấu ngoặc khi chạy lệnh man:  
- `1` chương trình thực thi hoặc lệnh shell  
- `2` Cuộc gọi hệ thống (các chức năng được cung cấp bởi kernel)  
- `3` cuộc gọi thư viện (chức năng trong thư viện chương trình)  
- `4` tệp đặc biệt (thường được tìm thấy trong /dev)  
- `5` Định dạng và quy ước về tệp, vd /etc/passwd  
- `6` trò chơi  
- `7` Khác (bao gồm các gói và quy ước vĩ mô), ví dụ: man(7)  
- `8` Lệnh quản trị hệ thống (thường chỉ dành cho root)  
- `9` Kernel routines [Không chuẩn]  

## 1.8 mandb
Lệnh `mandb` dùng để test trên Debian/Mint có tồn tại một trang man hay không.  
Hoặc trên CentOS ta chạy lệnh  
```
makewhatis
```
<a name ="2"></a>

# Phần 2. Các lệnh làm việc với thư mục  

## 2.1 pwd
Lệnh `pwd` dùng để hiển thị đường dẫn đến thư mục đang làm việc hiện tại  
```
[root@centos7srv network-scripts]# pwd
/etc/sysconfig/network-scripts
```
<a name= "3"></a>

## 2.2 cd
Bạn có thể thay đổi thư mục hiện tại đang làm việc bằng cách di chuyển tới thư mục khác qua lệnh `cd`.  
Các option đi kèm:  
- Thay đổi thư mục hiện tại sang thư mục cha
```
cd ..
```
- Thay đổi thư mục hiện tại về thư mục chính
```
cd ~
```
- Hiển thị thư mục làm việc cuối cùng từ nơi bạn di chuyển
```
cd --
```
- Di chuyển tới thư mục `home`
```
cd
```
<a name="4"></a>

## 2.3 ls
Lệnh `ls` dùng để hiển thị danh sách nội dung của thư mục  
```
[root@graylogsrv var]# ls
adm  cache  crash  db  empty  games  gopher  kerberos  lib  local  lock  log  mail  nis  opt  preserve  run  spool  tmp  yp
```
- Để hiển thị các thư mục và file ẩn
```
ls -a
.                        depmod.d                 hosts                     maven              .pwd.lock       statetab
..                       dhcp                     hosts.allow               mke2fs.conf        python          statetab.d
adjtime                  DIR_COLORS               hosts.deny                modprobe.d         rc0.d           subgid
aliases                  DIR_COLORS.256color      init.d                    modules-load.d     rc1.d           subuid
aliases.db               DIR_COLORS.lightbgcolor  inittab                   mongod.conf        rc2.d           sudo.conf

```
- Hiển thị thông tin đầy đủ như file hoặc thư mục con, size, modified date and time, file hoặc folder name và permission của nó.
```
ls -l
total 1068
-rw-r--r--.  1 root root              16 Dec 17 11:03 adjtime
-rw-r--r--.  1 root root            1518 Jun  7  2013 aliases
-rw-r--r--.  1 root root           12288 Dec 17 11:05 aliases.db
drwxr-xr-x.  2 root root            4096 Dec 21 09:42 alternatives
-rw-------.  1 root root             541 Apr 11  2018 anacrontab
-rw-r--r--.  1 root root              55 Oct 30  2018 asound.conf
drwxr-x---.  3 root root              43 Dec 17 10:59 audisp
drwxr-x---.  3 root root              83 Dec 17 11:05 audit
drwxr-xr-x.  2 root root              22 Dec 17 10:59 bash_completion.d
-rw-r--r--.  1 root root            2853 Oct 31  2018 bashrc
```
- Hiển thị size trong định dạng có thể đọc được.
```
ls -lh
total 1.1M
-rw-r--r--.  1 root root            16 Dec 17 11:03 adjtime
-rw-r--r--.  1 root root          1.5K Jun  7  2013 aliases
-rw-r--r--.  1 root root           12K Dec 17 11:05 aliases.db
drwxr-xr-x.  2 root root          4.0K Dec 21 09:42 alternatives
-rw-------.  1 root root           541 Apr 11  2018 anacrontab
-rw-r--r--.  1 root root            55 Oct 30  2018 asound.conf
drwxr-x---.  3 root root            43 Dec 17 10:59 audisp
drwxr-x---.  3 root root            83 Dec 17 11:05 audit
drwxr-xr-x.  2 root root            22 Dec 17 10:59 bash_completion.d
-rw-r--r--.  1 root root          2.8K Oct 31  2018 bashrc
```
- Sắp xếp File theo size
```
ls -lS
total 1068
-rw-r--r--.  1 root root          670293 Jun  7  2013 services
-rw-r--r--.  1 root root           20133 Dec 21 09:42 ld.so.cache
-rw-r--r--.  1 root root           12288 Dec 17 11:05 aliases.db
-rw-r--r--.  1 root root            7265 Dec 17 10:59 kdump.conf
-rw-r--r--.  1 root root            6722 Apr 11  2018 screenrc
-rw-r--r--.  1 root root            6545 Oct 31  2018 protocols
```
- Hiển thị danh sách thư mục dưới dạng tree
```
ls -R
```  
<a name="5"></a>

## 2.4 mkdir
- Lệnh `mkdir` (Make directory) được dùng để tạo thư mục  
```
mkdir folder1
```
- Tạo thư mục có chứa các thư mục con
```
mkdir -p /folder1/nga/nga1
```
<a name="6"></a>

## 2.4 rmdir
- Lệnh `rmdir` dùng để xóa thư mục trống (Remove Directory) 
```
rmdir nga1
```
- Để xóa các thư mục đệ quy sử dụng tùy chọn `-p`
```
rmdir -p /folder1/nga/nga1
```
<a name="7"></a>

# 3. Các lệnh làm việc với tệp

## 3.1 touch
Lệnh `touch` dùng để tạo một file trống
```
touch file1
```
- Tạo nhiều file
```
touch file1 file2 file3
```
- Tạo file chỉ định thời gian cụ thể
```
touch -t YYMMDDHHMM.SS file1
touch -t 1912242200.00 file1
```
- Tránh việc tạo file mới khi file không tồn tại
```
touch -c file1.txt
```

<a name="8"></a>

## 3.2 rm
Lệnh `rm` (remove) được dùng để xóa file  
```
rm file1
```
- Xóa nhiều file 
```
rm file1 file2 file3
```
- Nhắc trước khi xóa file
```
rm -i file1
```
Y: chấp nhận xóa
N: từ chối xóa
- Xóa thư mục đệ quy
```
rm -r 
```
- Xóa tập tin bất kể quyền hạn
```
rm -f
```
- Xóa bất kì thư mục nào (r - recursive: đệ quy, f - force) -> Bắt buộc xóa dù có các thư mục con và file bên trong.
```
rm -rf
```
### 3.3 file  
Lệnh `file` được dùng để xác định loại tệp  
Ví dụ  
```
[root@centos7srv ~]# file /etc/passwd
/etc/passwd: ASCII text

[root@centos7srv ~]# file /dev/sda
/dev/sda: block special

[root@centos7srv ~]# file -s /dev/sda
/dev/sda: x86 boot sector; partition 1: ID=0x83, active, starthead 32, startsector 2048, 2097152 sectors; partition 2: ID=0x8e, starthead 170, startsector 2099200, 39843840 sectors, code offset 0x63
```

<a name="9"></a>

### 3.4 cp  
Lệnh `cp` dùng để sao chép thư mục hoặc tệp tin  
- Sao chép đệ quy các thư mục  
```
cp -r
```
- Sao chép một file và đổi tên file
```
cp file1 file1.copy
```
- Sao chép 1 tệp vào 1 thư mục  
```
cp file1 dir1
```
- Sao chép nhiều tệp vào 1 thư mục  
```
cp file1 file2 file3 dir1/
```
- Xác nhận trước khi copy
```
cp -i file file1
```
- Copy file nhưng giữ lại toàn bộ thuộc tính của **file**

```
cp -p ./*.txt ./lab/
```
Lưu ý: Các thuộc tính được giữ lại là: access time, user ID, group ID, modification date

- Copy thư mục: sử dụng tùy chọn `-r` hoặc `-a`  
       - `-r`: copy toàn bộ file và các thư mục con của thư mục copy
       - `-a`: copy toàn bộ **thư mục** và duy trì các thuộc tính như timestamp, ownership

- Copy mà không ghi đè lên file đang có (file cùng tên) (n --no-clobber)
```
cp -n file1 folder1/
```
- Bắt buộc ghi đè
```
cp -f file1 folder1/
```
- Chỉ copy những phần chưa có trong file (u -update)
```
cp -u home/file1 /etc/file1
```
- Ghi đè file đang có ở thư mục đích
```
cp -i /etc/passwd /mnt/backup/
cp: overwrite '/mnt/backup/passwd'? y
```
- Tạo symbolic link  
```
root@mtd:~# cp -s /home/mtd/file_1.txt /mnt/backup/
root@mtd:~# cd /mnt/backup/
root@mtd:/mnt/backup# ls -l file_1.txt
lrwxrwxrwx 1 root root 27 Feb  5 18:37 file_1.txt -> /home/mtd/file_1.txt
root@mtd:/mnt/backup#
```
- Tạo hard link  
```
root@mtd:~# cp -l /home/mtd/devops.txt /mnt/backup/
root@mtd:~#
```
<a name="10"></a>

### 3.5 mv
Lệnh `mv` dùng để đổi tên file hoặc di chuyển file
- Tạo bản sao lưu (backup) với tệp đích hiện có
```
mv --backup[=CONTROL]
```
- Cũng tạo bản sao lưu nhưng không chấp nhận đối số truyền vào
```
mv -b
```
- Nhắc trước khi đổi tên
```
mv -i
```
- Không nhắc 
```
mv -f
```
- Không ghi đè lên 1 tập tin hiện có
```
mv -n
```
- Di chuyển tệp tin/thư mục
```
mv home/ngahong/file1 /home/ngakma
```
<a name="11"></a>

### 3.6 rename
Lệnh `rename` dùng để đổi tên file
- Diễn giải
```
rename -v
```
- Hiển thị phiên bản và thoát
```
rename -V
```
- Đổi tên peform trên taget liên kết mềm (symlink)
```
rename -s
```
Ví dụ
```
[root@centos7 ~]$ touch one.conf two.conf three.conf
[root@centos7 ~]$ rename .conf .backup *.conf
[root@centos7 ~]$ ls
one.backup three.backup two.backup
[root@centos7 ~]$
```
<a name="12"></a>

## 4. Các lệnh làm việc với nội dung file  

### 4.1 head  
Lệnh `head` dùng trong trường hợp ta muốn hiển thị phần đầu nội dung của file.  Mặc định lệnh `head` sẽ hiển thị 10 dòng đầu tiên của file  

```
[root@centos7srv ~]# head /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
```
- Ta cũng có thể hiển thị `n` dòng theo ý muốn 
```
[root@centos7srv ~]# head -4 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
```
- In ra 200 bytes đầu tiên của tệp
```
[root@centos7srv ~]# head -c 200 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/[root@centos7srv ~]#
```
- Không in ra tiêu đề xác định tên tệp (q- quiet)

```
head -q
```
-Luôn in tiêu đề xác định tên tệp ( v- verbose)
```
head -v
```

<a name="13"></a>

### 4.2 tail  

Lệnh `tail` dùng để hiển thị các dòng cuối của tệp. Mặc định nó sẽ hiển thị 10 dòng cuối cùng của file  

```
[root@centos7srv ~]# man head
[root@centos7srv ~]# tail /etc/passwd
nobody:x:99:99:Nobody:/:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:999:998:User for polkitd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
chrony:x:998:996::/var/lib/chrony:/sbin/nologin
nginx:x:997:995:Nginx web server:/var/lib/nginx:/sbin/nologin
mysql:x:27:27:MariaDB Server:/var/lib/mysql:/sbin/nologin
apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
```
- In ra `n` dòng cuối của file  
```
[root@centos7srv ~]# tail -n 3 /etc/passwd
nginx:x:997:995:Nginx web server:/var/lib/nginx:/sbin/nologin
mysql:x:27:27:MariaDB Server:/var/lib/mysql:/sbin/nologin
apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
```
- Tiếp tục đọc file cho đến khi có lệnh dừng (Ctrl+C) (f -follow)
```
tail -f
```
- In ra `n` bytes cuối của file  
```
tail -c 
```
- Không in ra tiêu đề của file
```
tail -q
```

<a name="14"></a>

### 4.3 Lệnh cat  

Lệnh `cat` dùng để hiển thị nội dung của tệp.  
```
[root@centos7srv ~]# cat song.txt
Song bat dau tu gio
Gio bat dau tu dau
Em cung khong biet nua
Khi nao ta yeu nhau
```

- `cat` là viết tắt của `concatenate`. Một trong những cách sử dụng cơ bản của `cat` là ghép các tệp thành một tệp lớn hơn (hoặc hoàn chỉnh).
```
[root@centos7srv ~]# echo line1 > f1
[root@centos7srv ~]# echo line2 > f2
[root@centos7srv ~]# echo line3 > f3
[root@centos7srv ~]# cat f1
line1
[root@centos7srv ~]# cat f2
line2
[root@centos7srv ~]# cat f3
line3
[root@centos7srv ~]# cat f1 f2 f3
line1
line2
line3
[root@centos7srv ~]# cat f1 f2 f3 > all
[root@centos7srv ~]# cat all
line1
line2
line3
```
- Có thể dùng cat để tạo các tập tin văn bản phẳng  
```
[root@centos7srv ~]# cat > muadong.txt
Mua dong lanh gia
Gio ret tung con
[root@centos7srv ~]# cat muadong.txt
Mua dong lanh gia
Gio ret tung con
```
Sau khi nhập dòng cuối thì ấn Ctrl+d để thoát.  

- Đánh dấu kết thúc tùy chỉnh  
- Copy tệp tin
```
[root@centos7srv ~]# cat song.txt
Song bat dau tu gio
Gio bat dau tu dau
Em cung khong biet nua
Khi nao ta yeu nhau
[root@centos7srv ~]# cat song.txt >> song2.txt
[root@centos7srv ~]# cat song2.txt
Song bat dau tu gio
Gio bat dau tu dau
Em cung khong biet nua
Khi nao ta yeu nhau
```
