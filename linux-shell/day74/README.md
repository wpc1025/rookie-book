# whereis(2019.08.16)

定位可执行文件、源代码文件、帮助文件在文件系统中的位置，这些文件的属性应属于原始代码，二进制文件或是帮助文件。还具有搜索源代码、指定备用搜索路径和搜索不寻常项的能力。

## 一、语法

`whereis [-bmsu] [BMS 目录名 -f ] 文件名`

## 二、参数

| 参数 | 作用 |
| :--- | :--- |
| -b | 定位可执行文件 |
| -m | 定位帮助文件 |
| -s | 定位源代码文件 |
| -u | 搜索默认路径下除可执行文件、源代码文件、帮助文件以外的其他文件 |
| -B | 指定搜索可执行文件的路径 |
| -M | 指定搜索帮助文件的路径 |
| -S | 指定搜索源代码文件的路径 |

## 三、实例

### 3.1 将和某文件相关的文件都查找出来

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ whereis git
git: /usr/bin/git /usr/share/man/man1/git.1.gz
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```

### 3.2 -b、-m、-s的应用

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ whereis -b git
git: /usr/bin/git
[rookie@izbp18hovh1qxijbodbas9z ~]$ whereis -m git
git: /usr/share/man/man1/git.1.gz
[rookie@izbp18hovh1qxijbodbas9z ~]$ whereis -s git
git:[rookie@izbp18hovh1qxijbodbas9z ~]$
```

参考博客地址：

[linux系列（十七）：whereis命令](https://www.cnblogs.com/felixwang2/p/10014916.html)