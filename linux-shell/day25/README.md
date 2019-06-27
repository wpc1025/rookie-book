# split命令(2019.06.28)

可以将大文件分割成较小的文件，在默认情况下将按照每1000行切割成一个小文件

## 一、语法

`split [参数] [切割文件] [文件名]`

## 二、常用参数

| 参数 | 作用 |
| :--- | :--- |
| -b | 指定每多少字节切成一个小文件 |
| -C | 与参数 -b 类似，但是在切割时将尽量维持每行的完整行 |

## 三、栗子

### 3.1 每六行分割成一个文件

    [rookie@izbp18hovh1qxijbodbas9z temp]$ split -6 log 
    [rookie@izbp18hovh1qxijbodbas9z temp]$ ls
    log  xaa  xab
    [rookie@izbp18hovh1qxijbodbas9z temp]$ less xaa 
    [rookie@izbp18hovh1qxijbodbas9z temp]$ less xab 
    [rookie@izbp18hovh1qxijbodbas9z temp]$ 
