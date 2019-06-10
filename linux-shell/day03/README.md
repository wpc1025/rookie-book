# locate命令(2019.06.06)

- `locate`命令用来查找文件或目录
- `locate`命令比`find -name`快，因为`locate`命令从数据库`/var/lib/mlocate/mlocate.db`中搜索，该数据库每天更新一次，可使用`updatedb`命令，手动更新数据库

## 命令格式

`locate [OPTION] ... [PATTERN]...`

## 命令参数

| 参数 | 作用 |
| :--- | :--- |
| -b | 仅将基本名称与指定的模式匹配，与`--wholename`相反 |
| -c | 只输出找到的数量 |
| -d | 使用`DBPATH`指定的数据库，而不是默认数据库 `/var/lib/mlocate/mlocate.db` |
| -i | 忽略大小写 |
| -w, --wholename | 匹配整个路径名（默认） |


## 栗子

### 搜索家目录下所有以wechat开头的文件

     [rookie@iZbp18hovh1qxijbodbas9Z ~]$ locate /home/rookie/wechat
     /home/rookie/wechat-spring-boot
     /home/rookie/wechat-spring-boot/logs
     /home/rookie/wechat-spring-boot/wechat-spring-boot.jar
     /home/rookie/wechat-spring-boot/logs/rookie.log
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-02-26
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-02-27
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-02-28
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-01
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-02
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-03
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-04
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-05
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-06
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-07
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-08
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-09
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-10
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-11
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-12
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-13
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-14
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-15
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-16
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-17
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-18
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-19
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-20
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-21
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-22
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-23
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-24
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-25
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-26
     /home/rookie/wechat-spring-boot/logs/rookie.log.2019-03-27
     [rookie@iZbp18hovh1qxijbodbas9Z ~]$

### 新增的文件无法locate，使用updatedb

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ touch new.txt
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ locate new.txt
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ sudo updatedb
    [sudo] password for rookie: 
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ locate new.txt
    /home/rookie/new.txt
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    

## 安装locate

学习过程中发现我的centos系统中locate命令没有安装，以下为安装步骤：

    sudo yum install mlocate
    sudo updatedb