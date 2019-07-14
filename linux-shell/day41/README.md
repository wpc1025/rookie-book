# echo命令(2019.07.14)

- 用于在终端设备中输出字符串或变量提取后的值

## 一、语法

`echo [参数] [字符串]`

## 二、常用参数

| 参数 | 作用 |
| :--- | :--- |
| -n | 不输出结尾的换行符 |
| -e"\a" | 发出警告音 |
| -e"\b" | 删除前面的一个字符 |
| -e"\c" | 结尾不加换行符 |
| -e"\f" | 换行，光标扔停留在原来的坐标位置 |
| -e"\n" | 换行，光标移至行首 |
| -e"\r" | 光标移至行首，但不换行 |
| -E | 禁止反斜杠转移，与-e参数功能相反 |


## 三、栗子

### 3.1 输出一段字符串

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ echo "hello rookie"
hello rookie
```

### 3.2 输出变量提取后的值

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ echo $PATH
/home/rookie/soft/node-v10.2.0-linux-x64/bin:/home/rookie/soft/hive-2.3.3/bin:/home/rookie/soft/hadoop-3.1.0/bin/:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/usr/java/jdk1.8.0_161/bin:/usr/java/jdk1.8.0_161/jre/bin:/home/rookie/.local/bin:/home/rookie/bin
[rookie@izbp18hovh1qxijbodbas9z ~]$
```

### 3.3 对内容进行转义，不让`$`符号的提取变量功能生效

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
[rookie@izbp18hovh1qxijbodbas9z ~]$ echo \$PATH
$PATH
[rookie@izbp18hovh1qxijbodbas9z ~]$
```

### 3.4 结合输出重定向，将字符串信息导入文件中

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ echo "this is a test" > rookie
[rookie@izbp18hovh1qxijbodbas9z ~]$ cat rookie 
this is a test
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```

### 3.5 使用反引号符执行命令，并输出结果到终端

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ echo `date`
Sun Jul 14 10:08:02 CST 2019
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```

### 3.6 输出带有换行符的内容

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ echo -e "a\nb\nc"
a
b
c
[rookie@izbp18hovh1qxijbodbas9z ~]$
```

### 3.7 输出信息中删除某个字符

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ echo -e "123\b456"
12456
[rookie@izbp18hovh1qxijbodbas9z ~]$
```
