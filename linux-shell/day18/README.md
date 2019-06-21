# chown命令(2019.06.21)

- 利用`chown`将指定文件的拥有者改为指定的用户或组，用户可以是用户名或者用户ID，组可以是组名或组ID
- 文件是以空格分开的要改变权限的文件列表，支持通配符

## 一、语法

`chown [-cfhvR] [--help] [--version] user[:group] file...`

### 1.1 参数

| 参数 | 作用 |
| :--- | :--- |
| user | 新的文件拥有者的使用者ID |
| group | 新的文件拥有者的使用组（group）|
| -c | 显示更改的部分的信息 |
| -f | 忽略错误信息 |
| -h | 修复符号链接 |
| -v | 显示详细的处理信息 |
| -R | 处理指定目录以及其子目录下的所有文件 |


## 二、栗子

**将文件的拥有者设为root**

    [root@iZbp18hovh1qxijbodbas9Z rookie]# ll | grep test.txt 
    -rw-rw-r--  1 rookie rookie        24 Jun 17 19:13 test.txt
    [root@iZbp18hovh1qxijbodbas9Z rookie]# chown root test.txt 
    [root@iZbp18hovh1qxijbodbas9Z rookie]# ll | grep test.txt 
    -rw-rw-r--  1 root   rookie        24 Jun 17 19:13 test.txt


**同时修改拥有者及所属组**

    [root@iZbp18hovh1qxijbodbas9Z rookie]# ll | grep test.txt 
    -rw-rw-r--  1 rookie rookie        24 Jun 17 19:13 test.txt
    [root@iZbp18hovh1qxijbodbas9Z rookie]# chown root:root test.txt 
    [root@iZbp18hovh1qxijbodbas9Z rookie]# ll | grep test.txt 
    -rw-rw-r--  1 root   root          24 Jun 17 19:13 test.txt
    [root@iZbp18hovh1qxijbodbas9Z rookie]# 


