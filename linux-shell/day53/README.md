# head命令(2019.07.26)

用来显示档案的开头至标准输出中，默认head命令打印其相应文件的开头10行

## 一、语法

`head [参数] [文件]`

## 二、参数

| 参数 | 作用 |
| :--- | :--- |
| -q | 隐藏文件名 |
| -v | 显示文件名 |
| -c <字节> | 显示字节数 |
| -n <行数> | 显示的行数 |

## 三、实例

### 3.1 显示文件的前n行

```
[rookie@izbp18hovh1qxijbodbas9z logs]$ head -n 5 rookie.log
```

### 3.2 显示文件的前n个字节

```
[rookie@izbp18hovh1qxijbodbas9z logs]$ head -c 20 rookie.log
```

### 3.3 显示文件除了最后n个字节之外的内容

```
[rookie@izbp18hovh1qxijbodbas9z logs]$ head -c -30 rookie.log
```

参考博客地址：

[linux命令（35）：head命令](https://www.cnblogs.com/yinjia/p/5467348.html)
