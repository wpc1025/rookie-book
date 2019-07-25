# kill命令(2019.07.25)

`kill`命令会向操作系统内核发送一个信号（多是终止信号）和目标进程的PID，然后系统内核根据收到的信号类型，对指定进程进行相应的操作。

## 一、语法

`kill [信号] PID`

## 二、信号列表

| 信号编号 | 信号名 | 含义 |
| :--- | :--- | :--- |
| 0 | EXIT | 程序退出时收到该信息 |
| 1 | HUP | 挂掉电话线或终端连接的挂起信号，这个信号会造成某些进程在没有终止的情况下重新初始化 |
| 2 | INT | 表示结束进程，但并不是强制性的，常用的“ctrl+c”组合键发出的就是一个`kill -2`的信号 |
| 3 | QUIT | 退出 |
| 9 | KILL | 杀死进程，即强制结束进程 |
| 11 | SEGV | 段错误 |
| 15 | TERM | 正常结束进程，是kill命令的默认信号 |

## 三、实例

### 3.1 标准kill命令

```
[root@izbp18hovh1qxijbodbas9z ~]# ps aux | grep wechat 
root     25863 14.7 12.6 2572528 237524 pts/0  Sl   15:41   0:19 java -jar wechat-spring-boot.jar --server.port=10005
root     25981  0.0  0.0 112660   964 pts/1    R+   15:44   0:00 grep --color=auto wechat
[root@izbp18hovh1qxijbodbas9z ~]# kill 25863
[root@izbp18hovh1qxijbodbas9z ~]# ps aux | grep wechat 
root     25992  0.0  0.0 112660   968 pts/1    R+   15:44   0:00 grep --color=auto wechat
[root@izbp18hovh1qxijbodbas9z ~]#
```

