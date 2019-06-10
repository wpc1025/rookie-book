# zip命令(2019.06.08)

- 压缩文件或目录

## 命令格式

`zip [选项] 压缩包名 源文件或源目录`

| 参数 | 作用 |
| :--- | :--- |
| -r | 压缩目录 |

## 栗子

### 压缩多个文件

    [root@iZbp18hovh1qxijbodbas9Z logs]# zip rookie-log.zip rookie.log.2019-03-2
    rookie.log.2019-03-20  rookie.log.2019-03-22  rookie.log.2019-03-24  rookie.log.2019-03-26  
    rookie.log.2019-03-21  rookie.log.2019-03-23  rookie.log.2019-03-25  rookie.log.2019-03-27  
    [root@iZbp18hovh1qxijbodbas9Z logs]# zip rookie-log.zip rookie.log.2019-03-2
    [root@iZbp18hovh1qxijbodbas9Z logs]# zip rookie-log.zip rookie.log.2019-03-25 rookie.log.2019-03-26 rookie.log.2019-03-27 
      adding: rookie.log.2019-03-25 (deflated 76%)
      adding: rookie.log.2019-03-26 (deflated 73%)
      adding: rookie.log.2019-03-27 (deflated 73%)
    [root@iZbp18hovh1qxijbodbas9Z logs]# ll | grep rookie-log.zip 
    -rw-r--r-- 1 root root  7578 Jun 10 18:22 rookie-log.zip
    [root@iZbp18hovh1qxijbodbas9Z logs]# 

### 压缩目录

    [root@iZbp18hovh1qxijbodbas9Z wechat-spring-boot]# zip -r rookie-log.zip logs/
      adding: logs/ (stored 0%)
      adding: logs/rookie.log.2019-03-06 (deflated 73%)
      adding: logs/rookie.log.2019-03-20 (deflated 73%)
      adding: logs/rookie.log.2019-03-18 (deflated 74%)
      adding: logs/rookie.log.2019-03-08 (deflated 73%)
      adding: logs/rookie.log.2019-03-04 (deflated 73%)
      adding: logs/rookie.log.2019-03-02 (deflated 73%)
      adding: logs/rookie.log.2019-03-27 (deflated 73%)
      adding: logs/rookie.log.2019-03-05 (deflated 73%)
      adding: logs/rookie.log.2019-03-01 (deflated 73%)
      adding: logs/rookie.log.2019-03-14 (deflated 75%)
      adding: logs/rookie.log.2019-02-26 (deflated 73%)
      adding: logs/rookie.log.2019-03-22 (deflated 73%)
      adding: logs/rookie.log.2019-03-10 (deflated 75%)
      adding: logs/rookie.log.2019-03-07 (deflated 73%)
      adding: logs/rookie.log.2019-03-24 (deflated 73%)
      adding: logs/rookie.log (deflated 71%)
      adding: logs/rookie.log.2019-03-19 (deflated 73%)
      adding: logs/rookie.log.2019-03-13 (deflated 73%)
      adding: logs/rookie.log.2019-03-21 (deflated 73%)
      adding: logs/rookie.log.2019-02-28 (deflated 73%)
      adding: logs/rookie.log.2019-03-15 (deflated 73%)
      adding: logs/rookie.log.2019-03-25 (deflated 76%)
      adding: logs/rookie.log.2019-03-12 (deflated 73%)
      adding: logs/rookie.log.2019-03-16 (deflated 73%)
      adding: logs/rookie.log.2019-02-27 (deflated 73%)
      adding: logs/rookie.log.2019-03-26 (deflated 73%)
      adding: logs/rookie.log.2019-03-09 (deflated 73%)
      adding: logs/rookie.log.2019-03-03 (deflated 75%)
      adding: logs/rookie.log.2019-03-23 (deflated 73%)
      adding: logs/rookie.log.2019-03-11 (deflated 73%)
      adding: logs/rookie.log.2019-03-17 (deflated 74%)
    [root@iZbp18hovh1qxijbodbas9Z wechat-spring-boot]# ll 
    total 39136
    drwxr-xr-x 2 root   root       4096 Jun 10 18:23 logs
    -rw-r--r-- 1 root   root      81259 Jun 10 18:23 rookie-log.zip
    -rw-rw-r-- 1 rookie rookie 39987831 Jun  7  2018 wechat-spring-boot.jar
    [root@iZbp18hovh1qxijbodbas9Z wechat-spring-boot]# 

