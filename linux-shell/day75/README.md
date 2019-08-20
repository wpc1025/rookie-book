# mkdir(2019.08.17)

创建新目录

## 一、语法

`mkdir [-mp] 目录名称`

## 二、选项

| 选项 | 作用 |
| :--- | :--- |
| -m | 配置文件权限，若不配置，则使用`umask`设置的默认权限 |
| -p | 帮助你直接将需要的目录（包括上一级目录）递回创建起来 |

## 三、实例

```
[rookie@izbp18hovh1qxijbodbas9z tmp]$ mkdir test
[rookie@izbp18hovh1qxijbodbas9z tmp]$ mkdir test1/test2/test3/test4
mkdir: cannot create directory ‘test1/test2/test3/test4’: No such file or directory
[rookie@izbp18hovh1qxijbodbas9z tmp]$ mkdir -p test1/test2/test3/test4
[rookie@izbp18hovh1qxijbodbas9z tmp]$ mkdir -m 711 test2
[rookie@izbp18hovh1qxijbodbas9z tmp]$ ll
total 16
drwxrwxr-x 2 rookie rookie 4096 Aug 19 16:33 test
drwxrwxr-x 3 rookie rookie 4096 Aug 19 16:33 test1
drwx--x--x 2 rookie rookie 4096 Aug 19 16:34 test2
```

