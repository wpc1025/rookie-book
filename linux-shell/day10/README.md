# grep命令(2019.06.13)

强大的文本搜索工具，能使用正则表达式搜索文本，并把匹配的行打印出来

## 一、命令格式

`grep [OPTIONS] PATTERN [FILE...]`

`grep [OPTIONS] [-e PATTERN -f FILE] [FILE...]`

## 二、命令参数

| 参数 | 作用 |
| :--- | :--- |
| -a | 将binary文件以text文件的方式搜寻数据 |
| -c | 计算找到 搜寻字符串 的次数 |
| -i | 忽略大小写的不同 |
| -n | 顺便输出行号 |
| -v | 反向选择 |
| --color=auto | 可以将找到的关键字部分加上颜色的显示 |
| -E | 用来扩展选项为正则表达式 |
| -r | 逐层遍历查找 |
| -e | 使用pattern作为模式，可多次指定 |
| -A | 显示匹配行及前面多少行 |
| -B | 显示匹配行及后面多少行 |
| -C | 显示匹配行前后多少行 |
| --include | 指定匹配的文件类型 |
| --exclude | 过滤不需要匹配的文件类型 |



## 三、栗子

### 3.1 基本操作

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ cat test.txt 
    I am rookie.
    I am learning linux.
    study hard, improve every day.
    I am from Henan.
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ grep I test.txt 
    I am rookie.
    I am learning linux.
    I am from Henan.
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
### 3.2 显示行号

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ grep -n I test.txt 
    1:I am rookie.
    2:I am learning linux.
    4:I am from Henan.
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$
    
### 3.3 匹配到的总行数

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ grep -c I test.txt 
    3
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$
    
### 3.4 正则表达式

#### 3.4.1 正则表达式基本使用方式

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ grep [abcd]m test.txt 
    I am rookie.
    I am learning linux.
    I am from Henan.
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 

#### 3.4.2 反向搜索

字符类的反向选择`[^]`：如果想要搜索到有 `n` 的行，但不想要 `n` 前有 `r` 

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ grep -n '[^r]n' test.txt 
    2:I am learning linux.
    4:I am from Henan.
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ grep -n 'n' test.txt 
    2:I am learning linux.
    4:I am from Henan.
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
#### 3.4.3 行首和行尾字节

`^`符号，在字符类符号（[]）之内和之外是不同的，在`[]`内代表反向选择，在`[]`外则代表定位在行首的意义

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ grep -n ^I test.txt 
    1:I am rookie.
    2:I am learning linux.
    4:I am from Henan.
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 

#### 3.4.4 任意一个字节`.`与重复字节`*`

- `.`：代表 一定有一个任意字节 的意思
- `*`：代表 重复前一个字符，0到无穷多次 的意思，为组合形态


    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ grep -n ...kie test.txt 
    1:I am rookie.
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$



