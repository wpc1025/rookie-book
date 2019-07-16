# look命令(2019.07.16)

`look`命令用于查询单词

## 一、语法

`look [-adf] [-t <字尾字符串>] [字首字符串] [字典文件]`


## 二、参数说明

| 参数 | 作用 |
| :--- | :--- |
| -a | 使用另一个字典文件web2，该文件也位于`/usr/dict`目录下 |
| -d | 只对比英文字母或数字，其余一概忽略，不予对比 |
| -f | 忽略字符大小写区别 |
| -t <字尾字符串> | 设置字尾字符串 |

## 三、栗子

### 3.1 查找testfile文件中以字母L开头的所有行

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ cat testfile 
HELLO LINUX!  
Linux test.
Linux is a free unix-type opterating system.  
This is a linux testfile.
 
[rookie@izbp18hovh1qxijbodbas9z ~]$ look L testfile 
Linux test.
Linux is a free unix-type opterating system.  
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```