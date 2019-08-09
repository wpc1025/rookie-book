# stat命令(2019.08.09)

`stat`命令用于显示文件的状态信息，`stat`命令的输出信息比`ls`命令的输出信息要更详细。

## 一、语法

`stat (选项) (参数)`

## 二、选项

| 选项 | 作用 |
| :--- | :--- |
| -L | 支持符号连接 |
| -f | 显示文件系统状态而非文件状态 |
| -t | 以简洁方式输出信息 |

## 三、参数

文件：指定要显示信息的普通文件或者文件系统对应的设备文件名

## 四、实例

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ ls -l test.txt 
-rw-rw-r-- 1 rookie rookie 41 Aug  6 16:53 test.txt
[rookie@izbp18hovh1qxijbodbas9z ~]$ stat test.txt 
  File: ‘test.txt’
  Size: 41        	Blocks: 8          IO Block: 4096   regular file
Device: fd01h/64769d	Inode: 663432      Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/  rookie)   Gid: ( 1000/  rookie)
Access: 2019-08-06 16:53:34.654019274 +0800
Modify: 2019-08-06 16:53:30.317839281 +0800
Change: 2019-08-06 16:53:30.347840525 +0800
 Birth: -
[rookie@izbp18hovh1qxijbodbas9z ~]$ stat -f test.txt 
  File: "test.txt"
    ID: f81e9a57bc274a7d Namelen: 255     Type: ext2/ext3
Block size: 4096       Fundamental block size: 4096
Blocks: Total: 10287952   Free: 8470067    Available: 7941709
Inodes: Total: 2621440    Free: 2460556
[rookie@izbp18hovh1qxijbodbas9z ~]$ stat -t test.txt 
test.txt 41 8 81b4 1000 1000 fd01 663432 1 0 0 1565081614 1565081610 1565081610 0 4096
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```