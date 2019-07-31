# groupdel命令(2019.07.31)

`groupdel`命令用于删除指定的工作组，本命令要修改的系统文件包括`/etc/group`和`/etc/gshadow`。若该群组中仍包括某些用户，则必须先删除这些用户后，方能删除群组。

## 一、语法

`groupdel (参数)`

## 二、参数

组：要删除的工作组名称

## 三、实例

```
[root@izbp18hovh1qxijbodbas9z ~]# groupadd wpc
[root@izbp18hovh1qxijbodbas9z ~]# groupdel wpc
[root@izbp18hovh1qxijbodbas9z ~]# 
```