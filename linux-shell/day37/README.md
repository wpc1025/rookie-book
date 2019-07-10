# ps命令(2019.07.10)

用于显示当前系统的进程状态，使用该命令可以确定有哪些进程正在运行和运行的状态、进程是否结束、进程有没有僵死等

## 一、语法格式

`ps [参数]`

## 二、常用参数

| 参数 | 作用 |
| :--- | :--- |
| -a | 显示所有终端机下执行的程序 |
| a | 显示现行终端机下的所有程序，包括其他用户的程序 |
| -A | 显示所有程序 |
| x | 显示所有程序，不以终端机来区分 |
| u | 以用户为主的格式来显示程序状况 |


`ps`命令的参数有很多，这里就不一一列举了

## 三、栗子

### 3.1 把所有进程显示处理

```

[rookie@izbp18hovh1qxijbodbas9z ~]$ ps -aux
[rookie@izbp18hovh1qxijbodbas9z ~]$ ps -A

```

### 3.2 把所有进程显示出来，并输出到临时文件中

```

[rookie@izbp18hovh1qxijbodbas9z ~]$ ps aux > ps.txt

```

### 3.3 查找特定进程信息

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ ps -ef | grep ssh
root      1293     1  0 Jun27 ?        00:00:00 /usr/sbin/sshd -D
root     26640  1293  0 09:27 ?        00:00:00 sshd: rookie [priv]
rookie   26642 26640  0 09:27 ?        00:00:00 sshd: rookie@pts/0
rookie   26694 26643  0 09:43 pts/0    00:00:00 grep --color=auto ssh
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```

### 3.4 显示指定用户信息

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ ps -u rookie
  PID TTY          TIME CMD
 1435 ?        00:15:38 java
26642 ?        00:00:00 sshd
26643 pts/0    00:00:00 bash
26695 pts/0    00:00:00 ps
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```

### 3.5 按内存资源的使用量对进程进行排序

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ ps aux | sort -rnk 4
```