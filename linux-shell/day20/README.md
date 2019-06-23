# free命令(2019.06.23)

- `free`命令能够用来显示系统中物理上的空闲和已用内存，还有交换内存，同时，也能显示被内核使用的缓冲和缓存。
- 通过分析文件`/proc/meminfo`收集以上信息

## 一、语法格式

`free [参数]`

## 二、常用参数

| 参数 | 作用 |
| :--- | :--- |
| -b | 以Byte显示内存使用情况 |
| -k | 以KB为单位显示内存使用情况 |
| -m | 以MB为单位显示内存使用情况 |
| -g | 以GB为单位显示内存使用情况 |
| -s | 持续显示内存 |
| -t | 显示内存使用总和 |

## 三、栗子

### 3.1 显示内存使用情况

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ free
                  total        used        free      shared  buff/cache   available
    Mem:        1883492      949904       82512         492      851076      726624
    Swap:             0           0           0
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
### 3.2 用MB显示内存使用情况

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ free -m
                  total        used        free      shared  buff/cache   available
    Mem:           1839         927          80           0         831         709
    Swap:             0           0           0
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
### 3.2 以KB显示内存使用情况

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ free -k
                  total        used        free      shared  buff/cache   available
    Mem:        1883492      949856       82512         492      851124      726652
    Swap:             0           0           0
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 

### 3.3 以总和的形式显示内存的使用信息

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ free -t
                  total        used        free      shared  buff/cache   available
    Mem:        1883492      949924       92672         492      840896      726580
    Swap:             0           0           0
    Total:      1883492      949924       92672


### 3.4 周期性查询内存使用情况

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ free -s 10
                  total        used        free      shared  buff/cache   available
    Mem:        1883492      949848       92716         492      840928      726644
    Swap:             0           0           0
    
                  total        used        free      shared  buff/cache   available
    Mem:        1883492      949832       92732         492      840928      726664
    Swap:             0           0           0


