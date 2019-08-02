# chgrp命令(2019.08.03)

`chgrp`命令用于将每个指定文件的所属组设置为指定值。若使用`--reference`，则将每个文件的所属组设置为与指定参考文件相同。

## 一、用法

```
chgrp [选项]... 用户组 文件...
或：chgrp [选项]... --reference=参考文件 文件...
```
## 二、选项

| 选项 | 作用 |
| :--- | :--- |
| -v | 显示指令执行过程 |
| -c | 效果类似`-v`参数，但仅回报更改的部分 |
| -f | 不显示错误信息 |
| -h | 只对符号连接的文件做修改，而不更改其他任何相关文件 |
| -R | 递归处理，将指定目录下的所有文件及子目录一并处理 |
| --reference=<参考文件或目录> | 把指定文件或目录的所属群组全部设成和参考文件或目录的所属群组相同 |

## 三、实例

### 3.1 递归修改文件夹的群组

```
[root@izbp18hovh1qxijbodbas9z rookie]# 
[root@izbp18hovh1qxijbodbas9z rookie]# ll | grep wechat-spring-boot
drwxrwxr-x  3 rookie rookie      4096 Jul 25 15:37 wechat-spring-boot
[root@izbp18hovh1qxijbodbas9z rookie]# chgrp -R wpc wechat-spring-boot/
[root@izbp18hovh1qxijbodbas9z rookie]# ll | grep wechat-spring-boot
drwxrwxr-x  3 rookie wpc         4096 Jul 25 15:37 wechat-spring-boot
[root@izbp18hovh1qxijbodbas9z rookie]#
```

### 3.2 将文件夹A修改成和文件夹B一样的群组

```
[root@izbp18hovh1qxijbodbas9z rookie]# ll | grep -E 'wechat-*|rookie-operator-tools$'
drwxrwxr-x  2 rookie rookie      4096 Jul 25 16:58 rookie-operator-tools
drwxrwxr-x  3 rookie wpc         4096 Jul 25 15:37 wechat-spring-boot
[root@izbp18hovh1qxijbodbas9z rookie]# chgrp -R --reference=wechat-spring-boot rookie-operator-tools
[root@izbp18hovh1qxijbodbas9z rookie]# ll | grep -E 'wechat-*|rookie-operator-tools$'
drwxrwxr-x  2 rookie wpc         4096 Jul 25 16:58 rookie-operator-tools
drwxrwxr-x  3 rookie wpc         4096 Jul 25 15:37 wechat-spring-boot
[root@izbp18hovh1qxijbodbas9z rookie]# 
```

### 3.3 所属组可以是ID

```
[root@izbp18hovh1qxijbodbas9z rookie]# ll | grep wechat*
drwxrwxr-x  3 rookie rookie      4096 Jul 25 15:37 wechat-spring-boot
[root@izbp18hovh1qxijbodbas9z rookie]# chgrp -R 0 wechat-spring-boot/
[root@izbp18hovh1qxijbodbas9z rookie]# ll | grep wechat*
drwxrwxr-x  3 rookie root        4096 Jul 25 15:37 wechat-spring-boot
[root@izbp18hovh1qxijbodbas9z rookie]# 
```