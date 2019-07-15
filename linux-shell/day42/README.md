# hostname命令(2019.07.15)

- 用来显示或设置主机名
- `hostname`命令设置主机名之后，重启之后会丢失该设置
- 修改`/etc/hosts`和`/etc/sysconfig/network`的相关内容，可永久修改主机名

## 一、参数

| 参数 | 作用 |
| :--- | :--- |
| -i | 显示IP地址 |
| -f | 显示全域名 |
| -a | 显示主机的别名 |
| -s | 显示短格式主机名 |

## 二、栗子

### 2.1 显示主机名

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ hostname
izbp18hovh1qxijbodbas9z
[rookie@izbp18hovh1qxijbodbas9z ~]$ echo $HOSTNAME
izbp18hovh1qxijbodbas9z
[rookie@izbp18hovh1qxijbodbas9z ~]$ grep jfht /etc/sysconfig/network
[rookie@izbp18hovh1qxijbodbas9z ~]$
```

### 2.2 显示IP地址

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ hostname -i
172.16.0.88
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```

### 2.3 修改主机名

```
[root@izbp18hovh1qxijbodbas9z ~]# hostname
izbp18hovh1qxijbodbas9z
[root@izbp18hovh1qxijbodbas9z ~]# hostname rookie-aliyun-vps
[root@izbp18hovh1qxijbodbas9z ~]# hostname
rookie-aliyun-vps
[root@izbp18hovh1qxijbodbas9z ~]#
```
