# wc命令

wc(Word Count) 统计指定文件中的字节数、字数、行数，并将统计结果显示输出

## 命令格式

`wc [选项] 文件`

## 命令参数

| 参数 | 作用 |
| :--- | :--- |
| -c | 统计字节数 |
| -l | 统计行数 |
| -m | 统计字符数 |
| -w | 统计字数，一个字被定义为由空格、跳格或换行字符分割的字符串 |
| -L | 打印最长行的长度 |

## 栗子

### 第一个栗子

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ cat wc.txt 
    rookie
    bob
    alice
    i am wpc.
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ wc wc.txt 
     4  6 27 wc.txt
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ wc -l wc.txt
    4 wc.txt
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ wc -w wc.txt 
    6 wc.txt
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ wc -m wc.txt 
    27 wc.txt
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ wc -L wc.txt 
    9 wc.txt
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    

### 第二个栗子

统计当前目录下的文件数

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ ll
    total 159308
    -rw-rw-r--  1 rookie rookie   9494003 Apr  4  2018 115813.txt
    -rw-r--r--  1 rookie rookie    561128 Jun 13  2018 CreateProductInfo.war
    -rw-rw-r--  1 rookie rookie  42136047 Jul  5  2018 geth-windows-amd64-1.8.12-37685930.exe
    -rw-rw-r--  1 rookie rookie 110900951 Jun 21  2018 gradler-4.8.1-all.zip
    drwxrwxr-x  2 rookie rookie      4096 Jun  3  2018 jenkins
    drwxrwxr-x  5 rookie rookie      4096 Mar 22 11:19 lcb
    -rw-rw-r--  1 rookie rookie         0 Mar 22 16:32 productList.json?pageNo=1.1
    drwxr-xr-x 19 rookie rookie      4096 Jun 29  2018 Python-3.7.0
    drwxrwxr-x  3 rookie rookie      4096 Jun 29  2018 pythonWorkSpace
    drwxrwxr-x  7 rookie rookie      4096 Jun  1  2018 soft
    -rw-rw-r--  1 rookie rookie        27 Jun  5 20:04 wc.txt
    drwxrwxr-x  3 rookie rookie      4096 Sep 25  2018 wechat-spring-boot
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ ll | wc -l
    13

