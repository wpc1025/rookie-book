# more命令(2019.07.24)

Linux more 命令，以一页一页的形式显示文件内容

## 一、语法

`more [-dlfpcsu] [-num] [+/pattern] [+linenum] [fileNames..]`

## 二、参数

| 参数 | 作用 |
| :--- | :--- |
| -num | 一次显示的行数 |
| +num | 从第num行开始显示 |
| filenames | 欲显示文档的内容，可为复数个数 |
| -s | 当遇到连续两行以上的空白行，就代换成一行的空白行 |
| -p | 不以卷动的方式显示每一页，而是先清除荧幕后再显示内容 |
| -c | 与-p类似，不同的是先显示内容再清除其他旧资料 |

## 三、示例

### 3.1 逐页显示 rookie.log 文档内容，如有连续两行以上空白行则以一行空白行显示。

```
[rookie@izbp18hovh1qxijbodbas9z logs]$ more -s rookie.log
```

### 3.2 从第 20 行开始显示 rookie.log 之文档内容。

```
[rookie@izbp18hovh1qxijbodbas9z logs]$ more +20 rookie.log
```