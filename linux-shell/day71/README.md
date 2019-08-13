# rmdir命令(2019.08.13)

用于删除空的目录

## 一、语法

`rmdir [-p] dirName`

## 二、参数

-p ： 是当子目录被删除后使它也成为空目录的话，则顺便一并删除

## 三、实例

### 3.1 删除空目录

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ rmdir md5sum-example/
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```

### 3.2 -p选项

在工作目录的temp目录中，删除名为logs的子目录，若logs子目录删除后，temp目录成为空目录，则temp也给删除掉

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ rmdir -p temp/logs/
```