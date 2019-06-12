# awk命令(2019.06.12)

## 概述

- 强大的文本分析工具
- 把文件逐行读入，以空格为默认分隔符将每行切片
- 最基本功能是在文件或者字符串中基于指定规则浏览和抽取信息，awk抽取信息后，才能进行其他文本操作

## 重点
1. 分割符
2. `BEGIN`、`END`的用法
3. 正则匹配
4. 内置变量
5. 自定义变量、条件语句、循环语句、数组


## 使用方法

`awk '{pattern + action}' {filename}`

- `pattern`表示AWK在数据中查找的内容，就是要表示的正则表达式，用斜杠括起来
- `action`是在找到匹配内容时所执行的一系列命令

## 栗子

### 显示最近登录的5个账号

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ last -n 5
    rookie   pts/0        114.240.55.235   Wed Jun 12 19:04   still logged in   
    rookie   pts/2        114.240.55.235   Tue Jun 11 17:15 - 17:45  (00:29)    
    rookie   pts/0        114.240.55.235   Tue Jun 11 15:57 - 19:00  (03:03)    
    rookie   pts/0        114.240.55.235   Mon Jun 10 15:51 - 19:43  (03:52)    
    rookie   pts/0        114.240.55.235   Thu Jun  6 16:18 - 17:57  (01:39)    
    
    wtmp begins Sun Oct 15 23:25:17 2017
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ last -n 5 | awk '{print $1}'
    rookie
    rookie
    rookie
    rookie
    rookie
    
    wtmp
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
awk按照分割符将记录划分域，`$0`表示所有域，`$1`表示第一个域。。。

### 栗子1

显示/etc/passwd的账户

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ cat /etc/passwd
    root:x:0:0:root:/root:/bin/bash
    bin:x:1:1:bin:/bin:/sbin/nologin
    daemon:x:2:2:daemon:/sbin:/sbin/nologin
    adm:x:3:4:adm:/var/adm:/sbin/nologin
    lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
    sync:x:5:0:sync:/sbin:/bin/sync
    shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
    halt:x:7:0:halt:/sbin:/sbin/halt
    mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
    operator:x:11:0:operator:/root:/sbin/nologin
    games:x:12:100:games:/usr/games:/sbin/nologin
    ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
    nobody:x:99:99:Nobody:/:/sbin/nologin
    systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
    dbus:x:81:81:System message bus:/:/sbin/nologin
    polkitd:x:999:997:User for polkitd:/:/sbin/nologin
    postfix:x:89:89::/var/spool/postfix:/sbin/nologin
    chrony:x:998:996::/var/lib/chrony:/sbin/nologin
    sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
    ntp:x:38:38::/etc/ntp:/sbin/nologin
    tcpdump:x:72:72::/:/sbin/nologin
    nscd:x:28:28:NSCD Daemon:/:/sbin/nologin
    rookie:x:1000:1000::/home/rookie:/bin/bash
    mysql:x:27:27:MySQL Server:/var/lib/mysql:/bin/false
    gitlab-runner:x:1001:1001:GitLab Runner:/home/gitlab-runner:/bin/bash
    nginx:x:997:995:nginx user:/var/cache/nginx:/sbin/nologin
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ cat /etc/passwd | awk -F : '{print $1}'
    root
    bin
    daemon
    adm
    lp
    sync
    shutdown
    halt
    mail
    operator
    games
    ftp
    nobody
    systemd-network
    dbus
    polkitd
    postfix
    chrony
    sshd
    ntp
    tcpdump
    nscd
    rookie
    mysql
    gitlab-runner
    nginx
    
### 栗子2

显示/etc/passwd的账户以及账户对应的shell

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ cat /etc/passwd | awk -F : '{print $1"\t"$7}'
    root	/bin/bash
    bin	/sbin/nologin
    daemon	/sbin/nologin
    adm	/sbin/nologin
    lp	/sbin/nologin
    sync	/bin/sync
    shutdown	/sbin/shutdown
    halt	/sbin/halt
    mail	/sbin/nologin
    operator	/sbin/nologin
    games	/sbin/nologin
    ftp	/sbin/nologin
    nobody	/sbin/nologin
    systemd-network	/sbin/nologin
    dbus	/sbin/nologin
    polkitd	/sbin/nologin
    postfix	/sbin/nologin
    chrony	/sbin/nologin
    sshd	/sbin/nologin
    ntp	/sbin/nologin
    tcpdump	/sbin/nologin
    nscd	/sbin/nologin
    rookie	/bin/bash
    mysql	/bin/false
    gitlab-runner	/bin/bash
    nginx	/sbin/nologin
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
### 栗子3

显示/etc/passwd的账户和账户对应的shell，账户与shell之间以逗号分割，在所有行添加列名name,shell,在最后一行添加rookie
- `BEGIN`、`END`的用法


    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ cat /etc/passwd | awk -F : 'BEGIN {print "name,shell"} {print $1,$7} END {print "rookie"}'
    name,shell
    root /bin/bash
    bin /sbin/nologin
    daemon /sbin/nologin
    adm /sbin/nologin
    lp /sbin/nologin
    sync /bin/sync
    shutdown /sbin/shutdown
    halt /sbin/halt
    mail /sbin/nologin
    operator /sbin/nologin
    games /sbin/nologin
    ftp /sbin/nologin
    nobody /sbin/nologin
    systemd-network /sbin/nologin
    dbus /sbin/nologin
    polkitd /sbin/nologin
    postfix /sbin/nologin
    chrony /sbin/nologin
    sshd /sbin/nologin
    ntp /sbin/nologin
    tcpdump /sbin/nologin
    nscd /sbin/nologin
    rookie /bin/bash
    mysql /bin/false
    gitlab-runner /bin/bash
    nginx /sbin/nologin
    rookie
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$

### 栗子4

搜索/etc/passwd有root关键字的所有行，并显示对应的shell

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ awk -F : '/^root/{print $1"\t"$7}' /etc/passwd
    root	/bin/bash
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$
    
## awk 内置变量

| 变量 | 描述 |
| :--- | :--- |
| ARGC | 命令行参数个数 |
| ARGV | 命令行参数排列 |
| ENVIRON | 支持队列中环境变量的使用 |
| FILENAME | awk浏览的文件名 |
| FS | 设置输入域分隔符，等价于命令行`-F`选项 |
| NF | 浏览记录的域的个数 |
| NR | 已读的记录数 |
| OFS | 输出域分隔符 |
| ORS | 输出记录分隔符 |
| RS | 控制记录分隔符 |

### 栗子5
统计/etc/passwd文件名，每行的行号，每行的列数，对应的完整行内容

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ awk -F : '{print "filename:"FILENAME ",linenumber:"NR "columns:"NF ",linecontent:"$0}' /etc/passwd
    filename:/etc/passwd,linenumber:1columns:7,linecontent:root:x:0:0:root:/root:/bin/bash
    filename:/etc/passwd,linenumber:2columns:7,linecontent:bin:x:1:1:bin:/bin:/sbin/nologin
    filename:/etc/passwd,linenumber:3columns:7,linecontent:daemon:x:2:2:daemon:/sbin:/sbin/nologin
    filename:/etc/passwd,linenumber:4columns:7,linecontent:adm:x:3:4:adm:/var/adm:/sbin/nologin
    filename:/etc/passwd,linenumber:5columns:7,linecontent:lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
    filename:/etc/passwd,linenumber:6columns:7,linecontent:sync:x:5:0:sync:/sbin:/bin/sync
    filename:/etc/passwd,linenumber:7columns:7,linecontent:shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
    filename:/etc/passwd,linenumber:8columns:7,linecontent:halt:x:7:0:halt:/sbin:/sbin/halt
    filename:/etc/passwd,linenumber:9columns:7,linecontent:mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
    filename:/etc/passwd,linenumber:10columns:7,linecontent:operator:x:11:0:operator:/root:/sbin/nologin
    filename:/etc/passwd,linenumber:11columns:7,linecontent:games:x:12:100:games:/usr/games:/sbin/nologin
    filename:/etc/passwd,linenumber:12columns:7,linecontent:ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
    filename:/etc/passwd,linenumber:13columns:7,linecontent:nobody:x:99:99:Nobody:/:/sbin/nologin
    filename:/etc/passwd,linenumber:14columns:7,linecontent:systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
    filename:/etc/passwd,linenumber:15columns:7,linecontent:dbus:x:81:81:System message bus:/:/sbin/nologin
    filename:/etc/passwd,linenumber:16columns:7,linecontent:polkitd:x:999:997:User for polkitd:/:/sbin/nologin
    filename:/etc/passwd,linenumber:17columns:7,linecontent:postfix:x:89:89::/var/spool/postfix:/sbin/nologin
    filename:/etc/passwd,linenumber:18columns:7,linecontent:chrony:x:998:996::/var/lib/chrony:/sbin/nologin
    filename:/etc/passwd,linenumber:19columns:7,linecontent:sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
    filename:/etc/passwd,linenumber:20columns:7,linecontent:ntp:x:38:38::/etc/ntp:/sbin/nologin
    filename:/etc/passwd,linenumber:21columns:7,linecontent:tcpdump:x:72:72::/:/sbin/nologin
    filename:/etc/passwd,linenumber:22columns:7,linecontent:nscd:x:28:28:NSCD Daemon:/:/sbin/nologin
    filename:/etc/passwd,linenumber:23columns:7,linecontent:rookie:x:1000:1000::/home/rookie:/bin/bash
    filename:/etc/passwd,linenumber:24columns:7,linecontent:mysql:x:27:27:MySQL Server:/var/lib/mysql:/bin/false
    filename:/etc/passwd,linenumber:25columns:7,linecontent:gitlab-runner:x:1001:1001:GitLab Runner:/home/gitlab-runner:/bin/bash
    filename:/etc/passwd,linenumber:26columns:7,linecontent:nginx:x:997:995:nginx user:/var/cache/nginx:/sbin/nologin

### 栗子6

使用`printf`代替`print`

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ awk -F : '{printf("filename:%10s,linenumber:%s,columns:%s,linecontent:%s\n",FILENAME,NR,NF,$0)}' /etc/passwd
    filename:/etc/passwd,linenumber:1,columns:7,linecontent:root:x:0:0:root:/root:/bin/bash
    filename:/etc/passwd,linenumber:2,columns:7,linecontent:bin:x:1:1:bin:/bin:/sbin/nologin
    filename:/etc/passwd,linenumber:3,columns:7,linecontent:daemon:x:2:2:daemon:/sbin:/sbin/nologin
    filename:/etc/passwd,linenumber:4,columns:7,linecontent:adm:x:3:4:adm:/var/adm:/sbin/nologin
    filename:/etc/passwd,linenumber:5,columns:7,linecontent:lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
    filename:/etc/passwd,linenumber:6,columns:7,linecontent:sync:x:5:0:sync:/sbin:/bin/sync
    filename:/etc/passwd,linenumber:7,columns:7,linecontent:shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
    filename:/etc/passwd,linenumber:8,columns:7,linecontent:halt:x:7:0:halt:/sbin:/sbin/halt
    filename:/etc/passwd,linenumber:9,columns:7,linecontent:mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
    filename:/etc/passwd,linenumber:10,columns:7,linecontent:operator:x:11:0:operator:/root:/sbin/nologin
    filename:/etc/passwd,linenumber:11,columns:7,linecontent:games:x:12:100:games:/usr/games:/sbin/nologin
    filename:/etc/passwd,linenumber:12,columns:7,linecontent:ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
    filename:/etc/passwd,linenumber:13,columns:7,linecontent:nobody:x:99:99:Nobody:/:/sbin/nologin
    filename:/etc/passwd,linenumber:14,columns:7,linecontent:systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
    filename:/etc/passwd,linenumber:15,columns:7,linecontent:dbus:x:81:81:System message bus:/:/sbin/nologin
    filename:/etc/passwd,linenumber:16,columns:7,linecontent:polkitd:x:999:997:User for polkitd:/:/sbin/nologin
    filename:/etc/passwd,linenumber:17,columns:7,linecontent:postfix:x:89:89::/var/spool/postfix:/sbin/nologin
    filename:/etc/passwd,linenumber:18,columns:7,linecontent:chrony:x:998:996::/var/lib/chrony:/sbin/nologin
    filename:/etc/passwd,linenumber:19,columns:7,linecontent:sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
    filename:/etc/passwd,linenumber:20,columns:7,linecontent:ntp:x:38:38::/etc/ntp:/sbin/nologin
    filename:/etc/passwd,linenumber:21,columns:7,linecontent:tcpdump:x:72:72::/:/sbin/nologin
    filename:/etc/passwd,linenumber:22,columns:7,linecontent:nscd:x:28:28:NSCD Daemon:/:/sbin/nologin
    filename:/etc/passwd,linenumber:23,columns:7,linecontent:rookie:x:1000:1000::/home/rookie:/bin/bash
    filename:/etc/passwd,linenumber:24,columns:7,linecontent:mysql:x:27:27:MySQL Server:/var/lib/mysql:/bin/false
    filename:/etc/passwd,linenumber:25,columns:7,linecontent:gitlab-runner:x:1001:1001:GitLab Runner:/home/gitlab-runner:/bin/bash
    filename:/etc/passwd,linenumber:26,columns:7,linecontent:nginx:x:997:995:nginx user:/var/cache/nginx:/sbin/nologin
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 


## awk 编程

### 变量和赋值

**统计/etc/passwd的账户人数**

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ awk '{count++;print$0;} END{print "user count is ",count}' /etc/passwd
    root:x:0:0:root:/root:/bin/bash
    bin:x:1:1:bin:/bin:/sbin/nologin
    daemon:x:2:2:daemon:/sbin:/sbin/nologin
    adm:x:3:4:adm:/var/adm:/sbin/nologin
    lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
    sync:x:5:0:sync:/sbin:/bin/sync
    shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
    halt:x:7:0:halt:/sbin:/sbin/halt
    mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
    operator:x:11:0:operator:/root:/sbin/nologin
    games:x:12:100:games:/usr/games:/sbin/nologin
    ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
    nobody:x:99:99:Nobody:/:/sbin/nologin
    systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
    dbus:x:81:81:System message bus:/:/sbin/nologin
    polkitd:x:999:997:User for polkitd:/:/sbin/nologin
    postfix:x:89:89::/var/spool/postfix:/sbin/nologin
    chrony:x:998:996::/var/lib/chrony:/sbin/nologin
    sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
    ntp:x:38:38::/etc/ntp:/sbin/nologin
    tcpdump:x:72:72::/:/sbin/nologin
    nscd:x:28:28:NSCD Daemon:/:/sbin/nologin
    rookie:x:1000:1000::/home/rookie:/bin/bash
    mysql:x:27:27:MySQL Server:/var/lib/mysql:/bin/false
    gitlab-runner:x:1001:1001:GitLab Runner:/home/gitlab-runner:/bin/bash
    nginx:x:997:995:nginx user:/var/cache/nginx:/sbin/nologin
    user count is  26
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$
    
**统计某个文件夹下的文件占用的字节数**

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ ls -l
    total 159308
    -rw-rw-r--  1 rookie rookie   9494003 Apr  4  2018 115813.txt
    -rw-r--r--  1 rookie rookie    561128 Jun 13  2018 CreateProductInfo.war
    -rw-rw-r--  1 rookie rookie  42136047 Jul  5  2018 geth-windows-amd64-1.8.12-37685930.exe
    -rw-rw-r--  1 rookie rookie 110900951 Jun 21  2018 gradler-4.8.1-all.zip
    drwxrwxr-x  2 rookie rookie      4096 Jun  3  2018 jenkins
    drwxrwxr-x  5 rookie rookie      4096 Mar 22 11:19 lcb
    drwxrwxr-x  3 rookie rookie      4096 Jun 11 15:58 practice-linux-shell
    drwxr-xr-x 19 rookie rookie      4096 Jun 29  2018 Python-3.7.0
    drwxrwxr-x  3 rookie rookie      4096 Jun 29  2018 pythonWorkSpace
    drwxrwxr-x  7 rookie rookie      4096 Jun  1  2018 soft
    drwxrwxr-x  4 rookie rookie      4096 Jun 10 18:28 wechat-spring-boot
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ ls -l | awk 'BEGIN {size=0;} {size=size+$5;} END{print "[end]size is ",size}'
    [end]size is  163120801
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 

**统计某个文件夹下的文件占用的字节数，以M为单位显示**

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ ls -l | awk 'BEGIN {size=0;} {size=size+$5;} END{print "[end]size is ",size/1024/1024,"M"}'
    [end]size is  155.564 M
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
### 条件语句

**统计某个文件夹下的文件占用的字节数，过滤4096大小的文件**

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ ls -l | awk 'BEGIN{size=0;print "[start] size is ",size} {if($5!=4096){size=size+$5;}} END{print "[end]size is ",size/1024/1024,"M"}'
    [start] size is  0
    [end]size is  155.537 M
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$

### 循环语句

支持while、do/while、for、break、continue

### 数组

awk中数组的下标可以是数字和字母，数组的下标通常被称为关键字

**显示/etc/passwd的账户**

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ awk -F : 'BEGIN{count=0;} {name[count] = $1;count++;} END{for(i=0;i<NR;i++) print i,name[i]}' /etc/passwd
    0 root
    1 bin
    2 daemon
    3 adm
    4 lp
    5 sync
    6 shutdown
    7 halt
    8 mail
    9 operator
    10 games
    11 ftp
    12 nobody
    13 systemd-network
    14 dbus
    15 polkitd
    16 postfix
    17 chrony
    18 sshd
    19 ntp
    20 tcpdump
    21 nscd
    22 rookie
    23 mysql
    24 gitlab-runner
    25 nginx
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 

















