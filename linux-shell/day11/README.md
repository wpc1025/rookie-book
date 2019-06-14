# passwd命令(2019.06.14)

- `passwd`命令用于设置用户的认证信息，包括用户密码、密码过期时间等
- 系统管理者可用它管理系统用户的密码
- 只有管理者可以指定用户名称，一般用户只能变更自己的密码

## 一、语法

`passwd (选项) [参数]`

## 二、选项

| 参数 | 作用 |
| :--- | :--- |
| -d | 删除密码，仅有系统管理者才能使用 |
| -f | 强制执行 |
| -k | 设置只有密码过期失效后，方能更新 |
| -l | 锁住密码 |
| -s | 列出密码的相关信息，仅有系统管理者才能使用 |
| -u | 解开已上锁的账号 |

## 三、栗子

### 3.1 管理员修改其他用户密码


    [root@iZbp18hovh1qxijbodbas9Z ~]# 
    [root@iZbp18hovh1qxijbodbas9Z ~]# passwd wpc
    Changing password for user wpc.
    New password: 
    BAD PASSWORD: The password is shorter than 8 characters
    Retype new password: 
    passwd: all authentication tokens updated successfully.
    [root@iZbp18hovh1qxijbodbas9Z ~]# 
    
### 3.2 普通用户修改自己的密码

    [wpc@iZbp18hovh1qxijbodbas9Z ~]$ passwd 
    Changing password for user wpc.
    Changing password for wpc.
    (current) UNIX password: 
    New password: 
    Retype new password: 
    passwd: all authentication tokens updated successfully.
    [wpc@iZbp18hovh1qxijbodbas9Z ~]$ 

### 3.3 锁定密码，让用户无法修改密码

    [root@iZbp18hovh1qxijbodbas9Z ~]# passwd -l wpc
    Locking password for user wpc.
    passwd: Success
    [root@iZbp18hovh1qxijbodbas9Z ~]# su wpc
    [wpc@iZbp18hovh1qxijbodbas9Z root]$ passwd 
    Changing password for user wpc.
    Changing password for wpc.
    (current) UNIX password: 
    passwd: Authentication token manipulation error

### 3.4 清除密码

    [root@iZbp18hovh1qxijbodbas9Z ~]# passwd -d wpc
    Removing password for user wpc.
    passwd: Success
    [root@iZbp18hovh1qxijbodbas9Z ~]# passwd -S wpc
    wpc NP 2019-06-14 0 99999 7 -1 (Empty password.)
    [root@iZbp18hovh1qxijbodbas9Z ~]# 

eg:
清除密码并不代表清除用户，清除密码相当于该用户可以直接登录系统
