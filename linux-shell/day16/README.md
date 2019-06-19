# nc命令(2019.06.19)

- `nc`能通过TCP和UDP在网络中读取数据。通过和其他工具结合和重定向，你可以在脚本中以多种方式使用它。
- `nc`所做的就是在两台电脑之间建立链接并返回两个数据流

## 一、命令安装

`yum install nmap-ncat -y`

## 二、命令格式

`nc [OPTIONS...] [hostname] [port]`

## 三、命令选项

| 选项 | 作用 |
| :--- | :--- |
| -z | 零IO模式，仅报告连接状态 |
| -v | 显示命令执行过程 |
| -n | 不要通过DNS解析主机名 |
| -u | 使用UDP而不是默认TCP |
| -l | 绑定并监听传入的连接 |


## 四、栗子

### 4.1 端口扫描

端口扫描经常被系统管理员和黑客用来发现一些机器上开放的端口，帮助他们识别系统中的漏洞

    $ nc -v -n -z 47.98.124.76 21-25
    nc: connect to 47.98.124.76 port 21 (tcp) failed: Connection timed out
    Connection to 47.98.124.76 22 port [tcp/*] succeeded!
    nc: connect to 47.98.124.76 port 23 (tcp) failed: Connection timed out
    nc: connect to 47.98.124.76 port 24 (tcp) failed: Connection timed out
    nc: connect to 47.98.124.76 port 25 (tcp) failed: Connection timed out
    
    $ nc -v 47.98.124.76 22
    Connection to 47.98.124.76 22 port [tcp/ssh] succeeded!
    SSH-2.0-OpenSSH_7.4
    
### 4.2 聊天服务器

服务端监听一个端口：

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ nc -l 1567
    
    
    
    hello

客户端连接端口并发送消息：

    $ nc 47.98.124.76 1567
    
    
    
    hello
    

### 4.3 文件传输

文件源端

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ nc -l 1567 < test.txt 
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
客户端

    $ ls -l
    总用量 782
    drwxr-xr-x+ 1 admin None      0 6月  12 09:40 accountapi日志
    -rw-r--r--  1 admin None 742207 6月   4 09:36 project.zip
    -rwxrwxrwx  1 admin None     61 6月  11 14:58 rename.sh
    drwxr-xr-x+ 1 admin None      0 6月  11 16:48 temp
    -rwxrwxrwx  1 admin None    306 6月  11 15:21 test.sh
    
    admin@CHJ-20190326FRK ~
    $ nc 47.98.124.76 1567 > test.txt
    
    admin@CHJ-20190326FRK ~
    $ less test.txt
    
    admin@CHJ-20190326FRK ~
    $ ls -l
    总用量 783
    drwxr-xr-x+ 1 admin None      0 6月  12 09:40 accountapi日志
    -rw-r--r--  1 admin None 742207 6月   4 09:36 project.zip
    -rwxrwxrwx  1 admin None     61 6月  11 14:58 rename.sh
    drwxr-xr-x+ 1 admin None      0 6月  11 16:48 temp
    -rwxrwxrwx  1 admin None    306 6月  11 15:21 test.sh
    -rw-r--r--  1 admin None     24 6月  19 10:33 test.txt
    
    admin@CHJ-20190326FRK ~
    $

### 4.4 目录传输

服务端：

    [rookie@iZbp18hovh1qxijbodbas9Z wechat-spring-boot]$ tar -cvf - logs/ | nc -l 1567
    logs/
    logs/rookie.log.2019-03-06
    logs/rookie_log.tar.bz2
    logs/rookie.log.2019-03-20
    logs/rookie.log.2019-03-18
    logs/rookie.log.2019-03-08
    logs/rookie.log.2019-03-04
    logs/rookie.log.2019-03-02
    logs/rookie.log.2019-03-27
    logs/rookie.log.2019-03-05
    logs/rookie.log.2019-03-01
    logs/rookie.log.2019-03-14
    logs/rookie.log.2019-02-26
    logs/rookie.log.2019-03-22
    logs/rookie.log.2019-03-10
    logs/rookie.log.2019-03-07
    logs/rookie.log.2019-03-24
    logs/rookie.log
    logs/rookie.log.2019-03-19
    logs/rookie.log.2019-03-13
    logs/rookie.log.2019-03-21
    logs/rookie.log.2019-02-28
    logs/rookie.log.2019-03-15
    logs/rookie.log.2019-03-25
    logs/rookie.log.2019-03-12
    logs/rookie.log.2019-03-16
    logs/rookie.log.2019-02-27
    logs/rookie.tar
    logs/rookie.log.2019-03-26
    logs/rookie.log.2019-03-09
    logs/rookie.log.2019-03-03
    logs/rookie.log.2019-03-23
    logs/rookie.log.2019-03-11
    logs/rookie.log.2019-03-17
    logs/rookie_log.tar.gz
    [rookie@iZbp18hovh1qxijbodbas9Z wechat-spring-boot]$
    
客户端：
    
    $ nc -n 47.98.124.76 1567 | tar -xvf -
    logs/
    logs/rookie.log.2019-03-06
    logs/rookie_log.tar.bz2
    logs/rookie.log.2019-03-20
    logs/rookie.log.2019-03-18
    logs/rookie.log.2019-03-08
    logs/rookie.log.2019-03-04
    logs/rookie.log.2019-03-02
    logs/rookie.log.2019-03-27
    logs/rookie.log.2019-03-05
    logs/rookie.log.2019-03-01
    logs/rookie.log.2019-03-14
    logs/rookie.log.2019-02-26
    logs/rookie.log.2019-03-22
    logs/rookie.log.2019-03-10
    logs/rookie.log.2019-03-07
    logs/rookie.log.2019-03-24
    logs/rookie.log
    logs/rookie.log.2019-03-19
    logs/rookie.log.2019-03-13
    logs/rookie.log.2019-03-21
    logs/rookie.log.2019-02-28
    logs/rookie.log.2019-03-15
    logs/rookie.log.2019-03-25
    logs/rookie.log.2019-03-12
    logs/rookie.log.2019-03-16
    logs/rookie.log.2019-02-27
    logs/rookie.tar
    logs/rookie.log.2019-03-26
    logs/rookie.log.2019-03-09
    logs/rookie.log.2019-03-03
    logs/rookie.log.2019-03-23
    logs/rookie.log.2019-03-11
    logs/rookie.log.2019-03-17
    logs/rookie_log.tar.gz
    
    admin@CHJ-20190326FRK ~
    $ ls
    accountapi日志  logs  project.zip  rename.sh  temp  test.sh  test.txt
    


此外，通过`nc`也可以加密通过网络发送的数据、克隆一个设备、打开一个shell、反向shell

参考博客地址：
[linux ncat命令](https://www.cnblogs.com/zzPrince/p/6842951.html)

