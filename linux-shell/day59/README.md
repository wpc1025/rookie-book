# groups命令(2019.08.01)

`groups`命令在标准输入输出上输出指定用户所在组的组成员，每个用户属于`/etc/passwd`中指定的一个组和`/etc/group`中指定的其他组

## 一、语法

`group (选项) (参数)`

## 二、参数

用户名：指定要打印所属工作组的用户名

## 三、实例

显示rookie用户所属的组：

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ groups rookie
rookie : rookie
[rookie@izbp18hovh1qxijbodbas9z ~]$ groups root
root : root
[rookie@izbp18hovh1qxijbodbas9z ~]$
```
