# cp命令(2019.07.19)

- 用来将一个或多个源文件或者目录复制到指定的目的文件或目录
- 支持复制多个文件，当一次复制多个文件时，目标文件参数必须是一个已经存在的目录

## 一、语法

`cp (选项) (参数)`

## 二、选项

| 选项 | 作用 |
| :--- | :--- |
| -a | 此参数效果和同时指定"-dpR"参数相同 |
| -d | 当复制符号连接时，把目录文件或目录也建立为符号连接，并指向与源文件或目录连接的原始文件或目录 |
| -f | 强制复制文件或目录，不论目标文件或目录是否存在 |
| -i | 覆盖既有文件之前先询问用户 |
| -l | 对源文件复制硬链接，而非复制文件 |
| -p | 保留源文件或目录的属性 |
| -R/r | 递归处理，将指定目录下的所有文件与子目录一并处理 |
| -s | 对源文件建立符号连接，而非复制文件 |
| -u | 只会在源文件的更改时间较目标文件更新时或是名称相互对应的目标文件并不存在时，才复制文件 |
| -S | 在备份文件时，用指定的后缀“SUFFIX”代替文件的默认后缀 |
| -b | 覆盖已存在的文件目标前将目标文件备份 |


## 三、参数

- 源文件：指定源文件列表。默认情况下，cp命令不能复制目录，如果要复制目录，则必须使用`-R`选项
- 目标文件：指定目标文件。当“源文件”为多个文件时，要求“目标文件”为指定的目录

## 四、实例

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ cp -r practice-linux-shell/ practice-linux-shell-back
```

