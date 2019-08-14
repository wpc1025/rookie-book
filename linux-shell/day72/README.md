# 数据流重导向(2019.08.14)

## 一、数据流重导向符号

- `\>` ：以覆盖的方式将【正确的数据】输出到指定的文件或者装置上
- `1>>` ： 以累加的方式将【正确的数据】输出到指定的文件或者装置上
- `2>` ：以覆盖的方式将【错误的数据】输出到指定的文件或者装置上
- `2>>` ： 以累加的方式将【错误的数据】输出到指定的文件或者装置上
- `/dev/null` ： 黑洞装置，可以吃掉任何导向该装置的信息
- `<` ： 将原本需要由键盘输入的数据，改由文件内容来取代
- `<<` ： 结束的输入字符

### 1.1 实例一
将`ll /usr/bin/`命令的执行结果输出到文件中

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ ll /usr/bin/ > ~/usr-bin-content
[rookie@izbp18hovh1qxijbodbas9z ~]$ ll ~/usr-bin-content 
-rw-rw-r-- 1 rookie rookie 53323 Aug 14 19:36 /home/rookie/usr-bin-content
```

### 1.2 实例二
将`stdout`与`stderr`分存到不同的文件中

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ find /home/ -name .bashrc > list_right 2> list_error
```

### 1.3 实例三
将错误的数据丢弃，屏幕上显示正确的数据

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ find /home/ -name .bashrc 2> /dev/null 
/home/rookie/.bashrc
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```

### 1.4 实例四
将命令的数据全部写入名为list的文件中

```
# 错误
[rookie@izbp18hovh1qxijbodbas9z ~]$ find /home/ -name .bashrc > list 2> list
[rookie@izbp18hovh1qxijbodbas9z ~]$ less list

# 正确
[rookie@izbp18hovh1qxijbodbas9z ~]$ find /home/ -name .bashrc > list 2>&1
[rookie@izbp18hovh1qxijbodbas9z ~]$ less list

# 正确
[rookie@izbp18hovh1qxijbodbas9z ~]$ find /home/ -name .bashrc &> list
[rookie@izbp18hovh1qxijbodbas9z ~]$ less list
```

### 1.5 实例五
用`stdin`取代键盘的输入以创建新文件的简单流程

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ cat > catfile < ~/.bashrc 
[rookie@izbp18hovh1qxijbodbas9z ~]$ ll catfile ~/.bashrc 
-rw-rw-r-- 1 rookie rookie 231 Aug 14 19:47 catfile
-rw-r--r-- 1 rookie rookie 231 Sep  7  2017 /home/rookie/.bashrc
[rookie@izbp18hovh1qxijbodbas9z ~]$
```

### 1.6 实例六
结束的输入字符

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ cat > catfile << "eof"
> hello
> linux
> rookie
> eof
```

## 二、命令运行的判断依据

- `;` ： 不考虑命令相关性的连续命令下达
- `&&` ： `cmd1 && cmd2` 若 cmd1 运行完毕且正确运行($?=0)，则开始运行 cmd2；若 cmd1 运行完毕且为错误 ($?≠0)，则 cmd2 不运行。
- `||` ：`cmd1 || cmd2` 若 cmd1 运行完毕且正确运行($?=0)，则 cmd2 不运行；若 cmd1 运行完毕且为错误 ($?≠0)，则开始运行 cmd2。

### 2.1 实例一
不考虑相关性的连续命令下达

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ rm list_right ; rm list_error
```

### 2.2 实例二
使用 ls 查阅目录 /tmp/abc 是否存在，若存在则用 touch 创建 /tmp/abc/hehe 

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ ls /tmp/abc && touch /tmp/abc/rookie
ls: cannot access /tmp/abc: No such file or directory
[rookie@izbp18hovh1qxijbodbas9z ~]$ mkdir /tmp/abc
[rookie@izbp18hovh1qxijbodbas9z ~]$ ls /tmp/abc && touch /tmp/abc/rookie
[rookie@izbp18hovh1qxijbodbas9z ~]$ ll /tmp/abc/rookie 
-rw-rw-r-- 1 rookie rookie 0 Aug 14 19:55 /tmp/abc/rookie
```

### 2.3 实例三
测试 /tmp/abc 是否存在，若不存在则予以创建，若存在就不作任何事情

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ ll /tmp/abc || mkdir /tmp/abc
ls: cannot access /tmp/abc: No such file or directory
[rookie@izbp18hovh1qxijbodbas9z ~]$ ll /tmp/abc/
total 0
[rookie@izbp18hovh1qxijbodbas9z ~]$
```

### 2.4 实例四
不清楚`/tmp/abc`是否存在，但就是要创建`/tmp/abc/hehe`文件

```
[rookie@izbp18hovh1qxijbodbas9z ~]$  ls /tmp/abc || mkdir /tmp/abc && touch /tmp/abc/hehe
[rookie@izbp18hovh1qxijbodbas9z ~]$ ll /tmp/abc/
total 0
-rw-rw-r-- 1 rookie rookie 0 Aug 14 19:58 hehe
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```

- 若`/tmp/abc`不存在故回传`$?≠0`，则 (2)因为`||`遇到非为`0`的`$?`故开始`mkdir /tmp/abc`，由于`mkdir /tmp/abc`会成功进行，所以回传`$?=0`(3)因为`&&`遇到`$?=0`故会运行`touch /tmp/abc/hehe`，最终`hehe`就被创建了；
- 若`/tmp/abc`存在故回传`$?=0`，则 (2)因为`||`遇到`0`的`$?`不会进行，此时`$?=0`继续向后传，故 (3)因为`&&`遇到`$?=0`就开始创建`/tmp/abc/hehe`了！最终`/tmp/abc/hehe`被创建起来。

![命令依序运行的关系示意图](./cmd_1.gif)


**向鸟哥致敬**  
[数据流重导向](http://cn.linux.vbird.org/linux_basic/0320bash_5.php)