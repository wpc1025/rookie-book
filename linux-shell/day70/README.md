# md5sum命令(2019.08.12)

用于计算一段内容的md5校验和，内容可以从文件中读取，也可以从标准流输入。甚至可以将结果保存到文件，在以后使用这个文件来校验文件是否被改变。

`md5sum`只计算文件的内容，不关注文件的元信息

## 简单使用

计算某个文件的md5值

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ md5sum example.txt 
6ba380dc80a9e76703fa1ee60b2f6dfa  example.txt
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```

## 校验文件是否改变

将计算结果存储到文件中，之后使用这个文件来校验文件是否发生了改变：

```
[rookie@izbp18hovh1qxijbodbas9z md5sum-example]$ md5sum example.txt >> example.txt.md5sum
[rookie@izbp18hovh1qxijbodbas9z md5sum-example]$ cat example.txt.md5sum 
6ba380dc80a9e76703fa1ee60b2f6dfa  example.txt
[rookie@izbp18hovh1qxijbodbas9z md5sum-example]$ md5sum -c example.txt.md5sum 
example.txt: OK
[rookie@izbp18hovh1qxijbodbas9z md5sum-example]$ vim example.txt
[rookie@izbp18hovh1qxijbodbas9z md5sum-example]$ md5sum -c example.txt.md5sum 
example.txt: FAILED
md5sum: WARNING: 1 computed checksum did NOT match
[rookie@izbp18hovh1qxijbodbas9z md5sum-example]$ 
```

## 从流读入

当不使用选项或者跟着`-`的时候，会从标准输入流`stdin`读取数据，可以使用流传递：

```
[rookie@izbp18hovh1qxijbodbas9z md5sum-example]$ echo hello | md5sum 
b1946ac92492d2347c6235b4d2611184  -
[rookie@izbp18hovh1qxijbodbas9z md5sum-example]$ 
```

也可以手动输入，手动输入的时候使用`ctrl+D`结束输入：

```
[rookie@izbp18hovh1qxijbodbas9z md5sum-example]$ md5sum 
hello
b1946ac92492d2347c6235b4d2611184  -
[rookie@izbp18hovh1qxijbodbas9z md5sum-example]$ 
```