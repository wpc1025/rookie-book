# cat命令(2019.07.18)

用于连接文件并打印到标准输出设备上

## 一、语法

`cat [参数] filename`

## 二、参数

```
-n 或 --number：由 1 开始对所有输出的行数编号。

-b 或 --number-nonblank：和 -n 相似，只不过对于空白行不编号。

-s 或 --squeeze-blank：当遇到有连续两行以上的空白行，就代换为一行的空白行。

-v 或 --show-nonprinting：使用 ^ 和 M- 符号，除了 LFD 和 TAB 之外。

-E 或 --show-ends : 在每行结束处显示 $。

-T 或 --show-tabs: 将 TAB 字符显示为 ^I。

-A, --show-all：等价于 -vET。

-e：等价于"-vE"选项；

-t：等价于"-vT"选项；
```

## 三、栗子

### 3.1 把textfile1的文档内容加上行号输入到textfile2这个文档里面

`cat -n textfile1 > textfile2`

### 3.2 把 textfile1 和 textfile2 的文档内容加上行号（空白行不加）之后将内容附加到 textfile3 文档里

`cat -b textfile1 textfile2 >> textfile3`

### 3.3 清空 /etc/test.txt 文档内容

`cat /dev/null > /etc/test.txt`

### 3.4 cat 制作镜像文件

`cat /dev/fd0 > OUTFILE`


