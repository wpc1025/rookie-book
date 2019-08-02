# groupmod命令(2019.08.02)

`groupmod`命令更改群组识别码或名称，需要更改群组的识别码或名称时，可用`groupmod`指令来完成这项工作。

## 一、语法

```
groupmod (选项) (参数)
```

## 二、选项

| 选项 | 作用 |
| :--- | :--- |
| -g <群组识别码> | 设置欲使用的群组识别码 |
| -o | 重复使用群组识别码 |
| -n <新群组名称> | 设置欲使用的群组名称 |

## 三、参数

组名：指定要修改的工作组名

## 四、实例

修改组名

```
[root@izbp18hovh1qxijbodbas9z ~]# groupadd linuxso
[root@izbp18hovh1qxijbodbas9z ~]# tail -1 /etc/group
linuxso:x:1003:
[root@izbp18hovh1qxijbodbas9z ~]# groupmod -n linux linuxso
[root@izbp18hovh1qxijbodbas9z ~]# tail -1 /etc/group
linux:x:1003:
[root@izbp18hovh1qxijbodbas9z ~]# 
```


