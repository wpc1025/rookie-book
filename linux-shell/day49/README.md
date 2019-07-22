# su命令(2019.07.22)

用于切换当前用户身份到其他用户身份，变更时需输入所要变更的用户账号和密码

## 一、语法

`su (选项) (参数)`

## 二、选项

| 选项 | 作用 |
| :--- | :--- |
| -c | 执行完指定的指令后，即恢复原来的身份 |
| -f | 适用于csh与tsch，使shell不用去读取启动文件 |
| -l | 改变身份时，也同时变更工作目录，以及HOME、SHELL、USER、logname。此外，也会变更PATH变量 |
| -m ， -p | 变更身份时，不要变更环境变量 |
| -s | 指定要执行的shell |

## 三、参数

用户：指定要切换身份的目标用户 

## 四、实例

### 4.1 变更账号为root并在执行ls指令后退出变回原使用者

```
[rookie@izbp18hovh1qxijbodbas9z wechat-spring-boot]$ su -c ls root
Password: 
logs  logs.tar	rookie.log  wechat-spring-boot.jar
```

### 4.2 变更帐号为root并改变工作目录至root的家目录

```
[root@izbp18hovh1qxijbodbas9z wechat-spring-boot]# su - root
Last login: Mon Jul 22 15:39:07 CST 2019 on pts/0
[root@izbp18hovh1qxijbodbas9z ~]# pwd
/root
[root@izbp18hovh1qxijbodbas9z ~]# 
```


        
        