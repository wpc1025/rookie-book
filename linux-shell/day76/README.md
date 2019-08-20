# id(2019.08.18)

id命令可以显示真实有效的用户ID(UID)和组ID(GID)。UID 是对一个用户的单一身份标识。组ID（GID）则对应多个UID。id命令已经默认预装在大多数Linux系统中。要使用它，只需要在你的控制台输入id。不带选项输入id会显示如下。结果会使用活跃用户。

## 语法

`id [-gGnru][--help][--version][用户名称]`

## 选项

| 选项 | 作用 |
| :--- | :--- |
| -g | 显示用户所属群组的ID |
| -G | 显示用户所属附加群组的ID |
| -n | 显示用户，所属群组或附加群组的命令 |
| -r | 显示实际ID |
| -u | 显示用户ID |

## 实例

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ id
uid=1000(rookie) gid=1000(rookie) groups=1000(rookie)
[rookie@izbp18hovh1qxijbodbas9z ~]$ id -a
uid=1000(rookie) gid=1000(rookie) groups=1000(rookie)
[rookie@izbp18hovh1qxijbodbas9z ~]$ id -G
1000
[rookie@izbp18hovh1qxijbodbas9z ~]$ id -g
1000
[rookie@izbp18hovh1qxijbodbas9z ~]$ id root
uid=0(root) gid=0(root) groups=0(root)
[rookie@izbp18hovh1qxijbodbas9z ~]$
```