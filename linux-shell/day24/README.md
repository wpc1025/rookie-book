# ifconfig命令(2019.06.27)

- 用于显示和配置Linux内核中网络接口的网络参数
- 用`ifconfig`配置的网卡信息，在网卡重启、机器重启后，配置就不存在，若想将上述的配置信息永远的存在电脑中，就要修改网卡的配置文件了

## 一、语法格式

`ifconfig [参数]`

## 二、常用参数

| 参数 | 作用 |
| :--- | :--- |
| add <地址> | 设置网络设备IPv6的IP地址 |
| del <地址> | 删除网络设备的IPv6的IP地址 |
| down | 关闭指定的网络设备 |
| up | 启动指定的网络设备 |
| IP地址 | 指定网络设备的IP地址 |

## 三、栗子

### 3.1 显示网络设备信息

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ ifconfig
    eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 172.16.0.88  netmask 255.255.240.0  broadcast 172.16.15.255
            ether 00:16:3e:0d:1a:2f  txqueuelen 1000  (Ethernet)
            RX packets 33561732  bytes 7270198487 (6.7 GiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 27559484  bytes 11198283705 (10.4 GiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
            inet 127.0.0.1  netmask 255.0.0.0
            loop  txqueuelen 1  (Local Loopback)
            RX packets 40930615  bytes 5747660787 (5.3 GiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 40930615  bytes 5747660787 (5.3 GiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
### 3.2 启动关闭指定网卡

    [root@linuxcool ~]# ifconfig eth0 down
    [root@linuxcool ~]# ifconfig eth0 up 

### 3.3 为网卡配置和删除IPv6地址

    [root@linuxcool ~]# ifconfig eth0 add 33ffe:3240:800:1005::2/64
    [root@linuxcool ~]# ifconfig eth0 del 33ffe:3240:800:1005::2/64


### 3.4 用`ifconfig`修改MAC地址
    
    [root@linuxcool ~]# ifconfig eth0 down
    [root@linuxcool ~]# ifconfig eth0 hw ether 00:AA:BB:CC:DD:EE
    [root@linuxcool ~]# ifconfig eth0 up
    [root@linuxcool ~]# ifconfig eth1 hw ether 00:1D:1C:1D:1E 
    [root@linuxcool ~]# ifconfig eth1 up

### 3.5 配置iP地址

    [root@linuxcool ~]# ifconfig eth0 192.168.1.56 
    [root@linuxcool ~]# ifconfig eth0 192.168.1.56 netmask 255.255.255.0
    [root@linuxcool ~]# ifconfig eth0 192.168.1.56 netmask 255.255.255.0 broadcast 192.168.1.255
