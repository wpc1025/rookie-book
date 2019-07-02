# lsof命令(2019.07.02)

- 查看文件的进程信息
- 查看进程打开的文件、打开文件的进程、进程打开的端口、找回/回复删除的文件

## 一、语法格式

`lsof [参数] [文件]`

## 二、参数

| 参数 | 作用 |
| :--- | :--- |
| -a | 列出打开文件存在的进程 |
| -c <进程名> | 列出指定进程打开的文件 |
| -g | 列出GID号进程详情 |
| -d <文件号> | 列出占用该文件号的进程 |
| +d <目录> | 列出目录被打开的文件 |
| +D <目录> | 递归列出目录下被打开的文件 |
| -n <目录> | 列出使用NFS的文件 |
| -i <条件> | 列出符合条件的进程 |
| -p <进程号> | 列出指定进程号所打开的文件 |
| -u | 列出UID号进程详情 |

## 三、栗子

### 3.1 查看文件的进程信息

    [root@izbp18hovh1qxijbodbas9z ~]# lsof | less
    COMMAND     PID  TID    USER   FD      TYPE             DEVICE  SIZE/OFF       NODE NAME
    systemd       1         root  cwd       DIR              253,1      4096          2 /
    systemd       1         root  rtd       DIR              253,1      4096          2 /
    systemd       1         root  txt       REG              253,1   1523568    1053845 /usr/lib/systemd/systemd
    systemd       1         root  mem       REG              253,1     20040    1050452 /usr/lib64/libuuid.so.1.3.0
    systemd       1         root  mem       REG              253,1    261336    1051899 /usr/lib64/libblkid.so.1.1.0
    systemd       1         root  mem       REG              253,1     90664    1050435 /usr/lib64/libz.so.1.2.7

### 3.2 列出GID号进程详情

    [root@izbp18hovh1qxijbodbas9z ~]# lsof | less
    COMMAND     PID  TID  PGID    USER   FD      TYPE             DEVICE  SIZE/OFF       NODE NAME
    systemd       1          1    root  cwd       DIR              253,1      4096          2 /
    systemd       1          1    root  rtd       DIR              253,1      4096          2 /
    systemd       1          1    root  txt       REG              253,1   1523568    1053845 /usr/lib/systemd/systemd
    systemd       1          1    root  mem       REG              253,1     20040    1050452 /usr/lib64/libuuid.so.1.3.0
    systemd       1          1    root  mem       REG              253,1    261336    1051899 /usr/lib64/libblkid.so.1.1.0
    systemd       1          1    root  mem       REG              253,1     90664    1050435 /usr/lib64/libz.so.1.2.7

### 3.3 列出目录下被打开的文件

    [root@izbp18hovh1qxijbodbas9z ~]# lsof +d /root
    COMMAND     PID  USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
    aliyun-se  1326  root  cwd    DIR  253,1     4096 131073 /root
    nginx      1511  root  cwd    DIR  253,1     4096 131073 /root
    nginx      1512 nginx  cwd    DIR  253,1     4096 131073 /root
    bash      10525  root  cwd    DIR  253,1     4096 131073 /root
    lsof      10553  root  cwd    DIR  253,1     4096 131073 /root
    lsof      10554  root  cwd    DIR  253,1     4096 131073 /root
    [root@izbp18hovh1qxijbodbas9z ~]# 
    
### 3.4 递归列出目录下被打开的文件

    [root@izbp18hovh1qxijbodbas9z ~]# lsof +D /home/rookie/ | less
    COMMAND   PID   USER   FD   TYPE DEVICE SIZE/OFF    NODE NAME
    java     1435 rookie  cwd    DIR  253,1     4096  655373 /home/rookie/soft/tomcat8
    java     1435 rookie  mem    REG  253,1       64  795940 /home/rookie/.jenkins/secret.key
    java     1435 rookie  mem    REG  253,1     4552  925069 /home/rookie/.jenkins/plugins/workflow-aggregator/WEB-INF/lib/workflow-aggregator.jar
    java     1435 rookie  mem    REG  253,1   315320 1063360 /home/rookie/.jenkins/plugins/email-ext/WEB-INF/lib/jsoup-1.8.3.jar

### 3.5 列出使用NFS的文件

    [root@izbp18hovh1qxijbodbas9z ~]# lsof -n /root
    COMMAND     PID  USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
    aliyun-se  1326  root  cwd    DIR  253,1     4096 131073 /root
    nginx      1511  root  cwd    DIR  253,1     4096 131073 /root
    nginx      1512 nginx  cwd    DIR  253,1     4096 131073 /root
    bash      10525  root  cwd    DIR  253,1     4096 131073 /root
    lsof      10566  root  cwd    DIR  253,1     4096 131073 /root
    lsof      10567  root  cwd    DIR  253,1     4096 131073 /root
    [root@izbp18hovh1qxijbodbas9z ~]# 

### 3.6 查看指定端口哪些进程在用

    [root@izbp18hovh1qxijbodbas9z ~]# lsof -i:80
    COMMAND    PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
    AliYunDun 1104  root   20u  IPv4  14281      0t0  TCP izbp18hovh1qxijbodbas9z:59664->100.100.30.26:http (ESTABLISHED)
    nginx     1511  root    6u  IPv4  18562      0t0  TCP *:http (LISTEN)
    nginx     1512 nginx    6u  IPv4  18562      0t0  TCP *:http (LISTEN)
    [root@izbp18hovh1qxijbodbas9z ~]# 
    
