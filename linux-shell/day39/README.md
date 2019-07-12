# fmt命令(2019.07.12)

- 从指定文件读取内容，按照指定格式重新编排后，输出到标准输出设备。
- 若指定的文件名为“-”，则`fmt`命令从标准输入设备读取数据

## 一、语法

`fmt [参数] [文件]`

## 二、常用参数

| 参数 | 作用 |
| :--- | :--- |
| -c | 每段前两列缩排 |
| -s | 只拆开字数超出每列字符数的列，但不合并字数不足每列字符数的列 |
| -t | 没列前两列缩排，但第1列与第2列的缩排格式不同 |
| -u | 每个字符之间都以一个空格字符间隔，每个句子之间则两个空格字符分割 |
| -w | 设置每列的最大字符数 |

## 三、栗子

### 3.1 重排指定文件

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ cat test.txt 
this is a good book.
you are a good man.
let us play basketball.
welcome to beijing.
建设美丽新家园.
[rookie@izbp18hovh1qxijbodbas9z ~]$ fmt test.txt 
this is a good book.  you are a good man.  let us play basketball.
welcome to beijing.  建设美丽新家园.
[rookie@izbp18hovh1qxijbodbas9z ~]$
```

### 3.2 将文件file重新排成10个字符一行

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ cat test.txt 
this is a good book.
you are a good man.
let us play basketball.
welcome to beijing.
建设美丽新家园.
[rookie@izbp18hovh1qxijbodbas9z ~]$ fmt -w 10 test.txt 
this is
a good
book.
you are a
good man.
let us
play
basketball.
welcome
to
beijing.
建设美丽新家园.
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```
