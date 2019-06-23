# df命令(2019.06.22)

- Disk Free，用于显示系统上可使用的磁盘空间
- 默认显示单位为KB，使用`df -h`的参数组合，根据磁盘容量自动变换合适的单位，更利于阅读

## 一、语法格式

`df [参数] [指定文件]`

## 二、参数格式

| 参数 | 作用 |
| :--- | :--- |
| -a | 显示所有系统文件 |
| -B <块大小> | 指定显示时的块大小 |
| -h | 以更容易阅读的方式显示 |
| -H | 以1000字节为换算单位来显示 | 
| -i | 显示索引字节信息 |
| -k | 指定块大小为1KB |
| -l | 只显示本地文件系统 |
| -t <文件系统类型> | 只显示指定类型的文件系统 |
| -T | 输出时显示文件系统类型 |
| --sync | 在取得磁盘使用信息前，先执行sync命令 |

## 三、栗子

### 3.1 显示磁盘分区使用情况

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ df
    Filesystem     1K-blocks    Used Available Use% Mounted on
    /dev/vda1       41151808 7487628  31550748  20% /
    devtmpfs          932240       0    932240   0% /dev
    tmpfs             941744       0    941744   0% /dev/shm
    tmpfs             941744     488    941256   1% /run
    tmpfs             941744       0    941744   0% /sys/fs/cgroup
    tmpfs             188352       0    188352   0% /run/user/1000
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 

### 3.2 以更容易阅读的方式显示磁盘分区使用情况

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ df -h
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/vda1        40G  7.2G   31G  20% /
    devtmpfs        911M     0  911M   0% /dev
    tmpfs           920M     0  920M   0% /dev/shm
    tmpfs           920M  488K  920M   1% /run
    tmpfs           920M     0  920M   0% /sys/fs/cgroup
    tmpfs           184M     0  184M   0% /run/user/1000
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$
    
### 3.3 显示指定文件所在分区的磁盘使用情况

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ df -h /home/rookie/
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/vda1        40G  7.2G   31G  20% /
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
### 3.4 显示文件类型为ext4的磁盘使用情况

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ df -ht ext4
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/vda1        40G  7.2G   31G  20% /
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
