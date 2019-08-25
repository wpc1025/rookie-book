# nmap(2019.08.23)

一款开放源代码的网络探测和安全审核工具，它的设计目标是快速的扫描大型网络

## 一、语法

`nmap (选项) (参数)`

## 二、选项

```
-O：激活操作探测；
-P0：值进行扫描，不ping主机；
-PT：是同TCP的ping；
-sV：探测服务版本信息；
-sP：ping扫描，仅发现目标主机是否存活；
-ps：发送同步（SYN）报文；
-PU：发送udp ping；
-PE：强制执行直接的ICMPping；
-PB：默认模式，可以使用ICMPping和TCPping；
-6：使用IPv6地址；
-v：得到更多选项信息；
-d：增加调试信息地输出；
-oN：以人们可阅读的格式输出；
-oX：以xml格式向指定文件输出信息；
-oM：以机器可阅读的格式输出；
-A：使用所有高级扫描选项；
--resume：继续上次执行完的扫描；
-P：指定要扫描的端口，可以是一个单独的端口，用逗号隔开多个端口，使用“-”表示端口范围；
-e：在多网络接口Linux系统中，指定扫描使用的网络接口；
-g：将指定的端口作为源端口进行扫描；
--ttl：指定发送的扫描报文的生存期；
--packet-trace：显示扫描过程中收发报文统计；
--scanflags：设置在扫描报文中的TCP标志。
```

## 三、参数

ip地址：指定待扫描报文的TCP地址

## 四、示例

安装`nmap`

```
bogon:~ hanfeige$ nmap www.baidu.com
Starting Nmap 7.80 ( https://nmap.org ) at 2019-08-25 21:31 CST
Nmap scan report for www.baidu.com (61.135.169.121)
Host is up (0.020s latency).
Other addresses for www.baidu.com (not scanned): 61.135.169.125
Not shown: 997 filtered ports
PORT    STATE  SERVICE
80/tcp  open   http
443/tcp open   https
593/tcp closed http-rpc-epmap

Nmap done: 1 IP address (1 host up) scanned in 5.07 seconds
bogon:~ hanfeige$
```