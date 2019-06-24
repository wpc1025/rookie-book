# uname命令(2019.06.25)

显示系统信息

## 一、语法格式

`uname [参数]`

## 二、常用参数

| 参数 | 作用 |
| :--- | :--- |
| -a | 显示系统所有相关信息 |
| -m | 显示计算机硬件架构 |
| -n | 显示主机名称 |
| -r | 显示内核发行版本号 |
| -s | 显示内核名称 |
| -v | 显示内核版本 |
| -p | 显示主机处理类型 |
| -o | 显示操作系统名称 |
| -i | 显示硬件平台 |

## 三、栗子

### 3.1 显示系统主机名、内核版本号、CPU类型等信息

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ uname -a
    Linux iZbp18hovh1qxijbodbas9Z 3.10.0-693.2.2.el7.x86_64 #1 SMP Tue Sep 12 22:26:13 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
### 3.2 仅显示系统主机名

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ uname -n
    iZbp18hovh1qxijbodbas9Z
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$
    
### 3.3 显示当前系统的内核版本

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ uname -r
    3.10.0-693.2.2.el7.x86_64
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
### 3.4 显示当前系统的硬件架构

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ uname -m
    x86_64
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 

