# unalias命令(2019.07.04)

- 取消命令别名

## 一、命令格式

`unalias [参数] [别名]`

## 二、常用参数

| 参数 | 作用 |
| :--- | :--- |
| -a | 取消所有命令别名 |

## 三、栗子

### 3.1 取消某个别名

    [rookie@izbp18hovh1qxijbodbas9z ~]$ alias hello='ls -al'
    [rookie@izbp18hovh1qxijbodbas9z ~]$ hello
    total 159444
    drwx------   21 rookie rookie      4096 Jul  3 17:57 .
    drwxr-xr-x.   5 root   root        4096 Jun 14 18:07 ..
    -rw-rw-r--    1 rookie rookie   9494003 Apr  4  2018 115813.txt
    -rw-rw-r--    1 rookie rookie        15 Jun 16 22:00 a
    [rookie@izbp18hovh1qxijbodbas9z ~]$ unalias hello
    [rookie@izbp18hovh1qxijbodbas9z ~]$ hello
    -bash: hello: command not found
    [rookie@izbp18hovh1qxijbodbas9z ~]$ 


