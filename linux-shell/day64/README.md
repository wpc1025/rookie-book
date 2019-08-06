# tac命令(2019.08.06)

将每个指定文件按行倒置并写出到标准输出

如果不指定文件，或文件为“-”，则从标准输入读取数据

## 一、语法

`tac [选项]... [文件]...`

## 二、选项

| 选项 | 作用 |
| :--- | :--- |
| -b | 在行前而非行尾添加分隔标志 |
| -r | 将分隔标志视作正则表达式来解析 |
| -s | 使用指定字符串代替换行作为分割标志 |

## 三、实例

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ tac test.txt 
you i he.
lalal
hello heihei.
hello vim.
[rookie@izbp18hovh1qxijbodbas9z ~]$ cat test.txt 
hello vim.
hello heihei.
lalal
you i he.
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```