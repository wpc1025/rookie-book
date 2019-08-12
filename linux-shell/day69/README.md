# pidof命令(2019.08.11)

用于找出包含指定名字进程的pid信息

## 一、语法

`pidof [-s] [-c] [-n] [-x] [-m] [-o omitpid[,omitpid..]]  [-o omitpid[,omitpid..]..]  program [program..]`

## 二、选项

| 选项 | 作用 |
| :--- | :--- |
| -s | 只返回一个pid |
| -o | 省略具有指定pid的进程 |

## 三、实例

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ pidof nginx
22470 1511
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```