# sort命令(2019.08.08)

将文本文件内容加以排序，`sort`可针对文本文件的内容，以行为单位来排序。

## 一、参数

| 参数 | 作用 |
| :--- | :--- |
| -b | 忽略每行前面开始出的空格字符 |
| -c | 检查文件是否已经按照顺序排序 |
| -d | 排序时，处理英文字母、数字及空格字符外，忽略其他的字符 |
| -f | 排序时，将小写字母视为大写字母 |
| -i | 排序时，除了040至176之间的ASCII字符外，忽略其他的字符 |
| -m | 将几个排序好的文件进行合并 |
| -M | 将前面3个字母依照月份的缩写进行排序 |
| -n | 依照数值的大小排序 |
| -o<输出文件> | 将排序后的结果存入指定的文件 |
| -r | 以相反的顺序排序 |
| -t<分割字符> | 指定排序时所用的栏位分割字符 |
| +<起始栏位>-<结束栏位> | 以指定的栏位来排序，范围由起始栏位到结束栏位的前一栏位 |

## 二、实例

### 2.1 实例一

`sort`将文件的每一行作为一个单位，相互比较，比较原则是从首字符向后，依次按ASCII码值进行比较，最后将它们按升序输出：

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ cat seq 
banana
apple
1111
3333
2222
[rookie@izbp18hovh1qxijbodbas9z ~]$ sort seq 
1111
2222
3333
apple
banana
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```

### 2.2 sort的-u选项

`-u`选项输出行中去除重复行

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ cat seq 
banana
apple
1111
3333
2222
apple
banana
[rookie@izbp18hovh1qxijbodbas9z ~]$ sort -u seq 
1111
2222
3333
apple
banana
[rookie@izbp18hovh1qxijbodbas9z ~]$
```

### 2.3 sort的-n选项

`sort`默认将数字按照字符来排序，使用`-n`选项依照数值的大小排序

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ sort number 
1
11
2
3
4
5
78
[rookie@izbp18hovh1qxijbodbas9z ~]$ sort -n number 
1
2
3
4
5
11
78
[rookie@izbp18hovh1qxijbodbas9z ~]$
```

### 2.4 sort的-t和-k选项

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ cat date 
2017-12-02
2017-01-09
2017-10-23
2017-04-24
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```

假如要以月份来排序，则可以使用`-t`和`-k`选项：  
-t<分割字符>：指定排序时所用的栏位分割字符   
-k ： 指定列数

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ sort -n -k 2 -t '-' date
2017-01-09
2017-04-24
2017-10-23
2017-12-02
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```

参考博客地址：
[linux中sort命令](https://www.cnblogs.com/fulucky/p/8022718.html)