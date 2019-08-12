# rename命令(2019.08.10)

`rename`命令重命名文件，可以使用正则表达式

## 一、语法

`rename [options] expression replacement file...`

## 二、选项

| 选项 | 作用 |
| :--- | :--- |
| -v | 将重命名的内容都打印到标准输出 |
| -n | 测试会重命名的内容，将结果都打印，但是并不真正执行重命名的过程 |
| -f | force会覆盖本地已经存在的文件 |
| -e | 比较复杂，可以通过该选项，写一些脚本来做一些复杂的事情 |

支持通配符：
```
? 可替代单个字符
* 可替代多个字符
```

## 三、实例

### 3.1 替换文件名中的特定字段

```
[rookie@izbp18hovh1qxijbodbas9z renameexample]$ 
[rookie@izbp18hovh1qxijbodbas9z renameexample]$ ll
total 0
-rw-rw-r-- 1 rookie rookie 0 Aug 12 13:48 a.txt
[rookie@izbp18hovh1qxijbodbas9z renameexample]$ rename a A a.txt 
[rookie@izbp18hovh1qxijbodbas9z renameexample]$ ll
total 0
-rw-rw-r-- 1 rookie rookie 0 Aug 12 13:48 A.txt
[rookie@izbp18hovh1qxijbodbas9z renameexample]$
```

### 3.2 修改文件后缀

```
[rookie@izbp18hovh1qxijbodbas9z renameexample]$ ll
total 0
-rw-rw-r-- 1 rookie rookie 0 Aug 12 13:52 a.html
[rookie@izbp18hovh1qxijbodbas9z renameexample]$ rename .html .php *
[rookie@izbp18hovh1qxijbodbas9z renameexample]$ ll
total 0
-rw-rw-r-- 1 rookie rookie 0 Aug 12 13:52 a.php
[rookie@izbp18hovh1qxijbodbas9z renameexample]$
```