# sed命令(2019.06.17)

- 流编辑器，文本处理工具，能够配合正则表达式使用
- 处理时，把当前处理的行存储在临时缓冲区中，接着用`sed`处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕
- `sed`主要用来自动编辑一个或多个文件，简化对文件的反复操作，编写转换程序等

## 一、命令格式

`sed [选项] [动作]`

### 1.1 选项

| 选项 | 作用 |
| :--- | :--- |
| -n | 使用安静模式，在一般`sed`用法中，所有来自`STDIN`的数据一般都会被列出到终端上。加上`-n`参数后，则只有经过`sed`特殊处理的那一行才会被列出来 |
| -e | 直接在命令列模式上进行`sed`的动作编辑 |
| -f | 直接在命令列模式上进行`sed`的动作编辑 |
| -i | 直接修改读取的文件内容，而不是输出到终端 |

### 1.2 动作

| 动作 | 作用 |
| :--- | :--- |
| a | 新增行 |
| c | 取代行 |
| d | 删除行 |
| i | 插入行 |
| p | 列印 |
| s | 替换 |

- 一般`function`的前面会有一个地址的限制，例如`[地址]function`，表示我们的动作要进行的行

## 二、栗子

### 2.1 删除行

文件 test.txt 内容如下：

    11 aa
    22 bb
    33 cc
    23 dd
    55 2e
    
执行命令如下：

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ sed '1,2d' test.txt 
    33 cc
    23 dd
    55 2e

其中`1,2d`中的`d`代表删除，而`d`前面的表示删除的行的地址，而`1,2`表示一个地址范围，也就是删除第一行和第二行。  
`m,$d`就是删除`m`行以及其后面的所有行的内容。  
还可以使用正则表达式选出符合条件的行，并对这些行进行操作，例如删除所有包含2的行：

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ sed '/2/d' test.txt 
    11 aa
    33 cc
    
只想删除以2开头的行：

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ sed '/^2/d' test.txt 
    11 aa
    33 cc
    55 2e
    
### 2.2 在指定行后新增一行

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ sed '1a hello rookie' test.txt 
    11 aa
    hello rookie
    22 bb
    33 cc
    23 dd
    55 2e
    
### 2.3 在指定行前插入一行

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ sed '1i hello rookie' test.txt 
    hello rookie
    11 aa
    22 bb
    33 cc
    23 dd
    55 2e
    
### 2.4 替换行

#### 2.4.1 不使用正则

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ sed '1c hello rookie' test.txt 
    hello rookie
    22 bb
    33 cc
    23 dd
    55 2e

#### 2.4.2 使用正则

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ sed '/^2/c hello rookie' test.txt 
    11 aa
    hello rookie
    33 cc
    hello rookie
    55 2e
    
### 2.5 替换部分字符串而不是整行

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ sed 's/aa/AA/' test.txt 
    11 AA
    22 bb
    33 cc
    23 dd
    55 2e
    

`s/待替换的字符串/新字符串/`

`g`参数表示对一行里面所有的符合条件的字符串都做替换操作

### 2.6 搜索并输出行的内容

`p`命令用于搜索符合条件的行，并输出该行的内容，而不做任何的修改

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ sed -n '2p' test.txt 
    22 bb
    
### 2.7 将修改应用到文件中

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ sed -i '2d' test.txt 
    
### 2.8 sed正则中的元字符串

    $ 表示行尾 
    ^ 表示行首
    [a-z0-9]表示字符范围
    [^]表示除了字符集中的字符以外的字符 
    
    sed的正则中  \(\)  和 \{m,n\} 需要转义 
    . 表示任意字符  
    * 表示零个或者多个  
    \+ 一次或多次　　
    \? 零次或一次    
    \| 表示或语法
    
    
参考博客：
[linux sed命令就是这么简单](https://www.cnblogs.com/wangqiguo/p/6718512.html)