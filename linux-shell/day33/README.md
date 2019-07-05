# useradd命令(2019.07.06)

- 用来创建或更新用户信息

## 一、命令语法

`useradd [option] username`

## 二、选项

| 参数 | 作用 |
| :--- | :--- |
| -d <登入目录> | 指定用户登录时的目录 |
| -g <群组> | 初始群组 |
| -G <群组> | 非初始群组 |
| -m | 自动创建用户的家目录 |
| -M | 不要创建用户的家目录 |
| -N | 不要创建以用户名称为名的群组 |
| -s | 指定用户登入后所使用的shell |

## 三、栗子

### 3.1 添加新用户

    [root@izbp18hovh1qxijbodbas9z ~]# useradd test
    [root@izbp18hovh1qxijbodbas9z ~]#
    
### 3.2 不创建家目录，并且禁止登录

    [root@izbp18hovh1qxijbodbas9z ~]# useradd -M -s /sbin/nologin test
    [root@izbp18hovh1qxijbodbas9z ~]#
    
### 3.3 添加新用户，指定UID，归属用户组

    [root@izbp18hovh1qxijbodbas9z ~]# useradd -u 888 -s /bin/sh -G root linuxcool
    [root@izbp18hovh1qxijbodbas9z ~]# 
    


