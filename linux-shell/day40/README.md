# netstat命令(2019.07.13)

`netstat`命令用于显示各种网络相关信息，如网络连接、路由表、接口状态等等

## 一、常见参数

| 参数 | 作用 |
| :--- | :--- |
| -a | 显示所有选项 |
| -t | 仅显示tcp相关选项 |
| -u | 仅显示udp相关选项 |
| -n | 拒绝显示别名，能显示数字的全部转换为数字 |
| -l | 仅列出有在Listen状态的服务 |
| -p | 显示建立相关连接的程序名 |
| -r | 显示路由信息、路由表 |
| -e | 显示扩展信息，例如uid等 |
| -s | 按各个协议进行统计 |
| -c | 每隔一个固定时间，执行该netstat命令 |

## 二、栗子

### 3.1 列出所有端口

```
列出所有端口:     netstat -a
列出所有tcp端口:  netstat -at
列出所有udp端口:  netstat -au
```

### 3.2 列出所有处于监听状态的Sockets

```
只显示监听端口:          netstat -l
只列出所有监听tcp端口:   netstat -lt
只列出所有监听udp端口:   netstat -lu
只列出所有监听UNIX端口:  netstat -lx
```

### 3.3 显示每个协议的统计信息

```
显示所有端口的统计信息 netstat -s
显示TCP端口的统计信息 netstat -st 
显示UDP端口的统计信息 netstat -su
```

### 3.4 显示PID和进程名称

`netstat -pt`

### 3.5 不显示主机端口和用户名

```
netstat -an
```

### 3.6 持续输出netstat信息

```
netstat -t -c 2
```

### 3.7 找出程序运行的端口

```
netstat -apn | grep ssh
```






