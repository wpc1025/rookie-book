# which命令

在`PATH`变量指定的路径中，搜索某个系统命令的位置，并返回第一个搜索结果

## 命令格式

`which [options] programname`

## 命令参数

| 参数 | 作用 |
| :--- | :--- |
| -a | 打印出PATH中的所有匹配项，而不是仅仅第一个 |
| --skip-dot | 跳过PATH中以点开头的目录 |
| --skip-tilde | 跳过PATH中以波浪号开头的目录 |

## 栗子

### 第一个栗子

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ which java
    /usr/java/jdk1.8.0_161/bin/java
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ which --all java
    /usr/java/jdk1.8.0_161/bin/java
    /usr/java/jdk1.8.0_161/jre/bin/java
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$
    

