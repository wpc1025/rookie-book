# ln命令(2019.06.30)

用于将一个文件创建链接，分为软连接（类似windows的快捷方式）和硬链接（相当于对源文件的copy），命令默认使用硬链接。

## 一、语法

`ln [选项] [文件]`

## 二、栗子

### 2.1 对文件创建软链接

    [rookie@izbp18hovh1qxijbodbas9z ~]$ ln -s test.txt test-ln.txt 
    [rookie@izbp18hovh1qxijbodbas9z ~]$ ll
    total 159324
    -rw-rw-r--  1 rookie rookie   9494003 Apr  4  2018 115813.txt
    -rw-rw-r--  1 rookie rookie        15 Jun 16 22:00 a
    -rw-rw-r--  1 rookie rookie        15 Jun 16 22:11 b
    -rw-r--r--  1 rookie rookie    561128 Jun 13  2018 CreateProductInfo.war
    -rw-rw-r--  1 rookie rookie  42136047 Jul  5  2018 geth-windows-amd64-1.8.12-37685930.exe
    -rw-rw-r--  1 rookie rookie 110900951 Jun 21  2018 gradler-4.8.1-all.zip
    drwxrwxr-x  2 rookie rookie      4096 Jun  3  2018 jenkins
    drwxrwxr-x  5 rookie rookie      4096 Mar 22 11:19 lcb
    drwxrwxr-x  3 rookie rookie      4096 Jun 11 15:58 practice-linux-shell
    drwxr-xr-x 19 rookie rookie      4096 Jun 29  2018 Python-3.7.0
    drwxrwxr-x  3 rookie rookie      4096 Jun 29  2018 pythonWorkSpace
    drwxrwxr-x  7 rookie rookie      4096 Jun  1  2018 soft
    drwxrwxr-x  2 rookie rookie      4096 Jun 27 17:34 temp
    lrwxrwxrwx  1 rookie rookie         8 Jun 29 23:43 test-ln.txt -> test.txt
    -rw-rw-r--  1 rookie rookie         6 Jun 29 19:57 test.txt
    drwxrwxr-x  3 rookie rookie      4096 Jun 19 10:38 wechat-spring-boot
    [rookie@izbp18hovh1qxijbodbas9z ~]$ 



