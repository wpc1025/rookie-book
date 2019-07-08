# file命令(2019.07.08)

- `file`命令可以用来识别文件类型、辨别一些文件的编码格式
- 通过查看文件的头部信息来获取文件类型，而不是像windows通过扩展名来确定文件类型

## 一、语法

`file [参数] [文件]`

## 二、参数

| 参数 | 作用 |
| :--- | :--- |
| -b | 列出辨识结果，不显示文件名称 |
| -c | 详细显示指令执行过程 |
| -f | 指定名称文件，其内容有一个或多个文件名称时，让file依序辨识这些文件，格式为每列一个文件名称 |
| -L | 直接显示符号链接所指向的文件类别 |
| -i | 显示MIME类别 |

## 三、栗子

### 3.1 显示文件类型

    [rookie@izbp18hovh1qxijbodbas9z ~]$ file test.txt 
    test.txt: ASCII text
    [rookie@izbp18hovh1qxijbodbas9z ~]$ 
    
### 3.2 显示文件类型，不显示文件名称

    [rookie@izbp18hovh1qxijbodbas9z ~]$ file -b test.txt 
    ASCII text
    [rookie@izbp18hovh1qxijbodbas9z ~]$
    
### 3.3 显示文件类型，显示MIME类别，不显示文件名称

    [rookie@izbp18hovh1qxijbodbas9z ~]$ file -b -i test.txt 
    text/plain; charset=us-ascii
    [rookie@izbp18hovh1qxijbodbas9z ~]$
    
### 3.4 显示符号链接的文件类型

    [rookie@izbp18hovh1qxijbodbas9z ~]$ ls -l test-ln.txt 
    lrwxrwxrwx 1 rookie rookie 8 Jun 29 23:43 test-ln.txt -> test.txt
    [rookie@izbp18hovh1qxijbodbas9z ~]$ file test-ln.txt 
    test-ln.txt: symbolic link to `test.txt'
    [rookie@izbp18hovh1qxijbodbas9z ~]$ 
    
### 3.5 显示符号链接所指向的文件类别

    [rookie@izbp18hovh1qxijbodbas9z ~]$ file -L test-ln.txt 
    test-ln.txt: ASCII text
    [rookie@izbp18hovh1qxijbodbas9z ~]$
