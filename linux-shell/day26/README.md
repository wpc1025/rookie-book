# tee命令(2019.06.29)

读取标准输入的数据，并将其内容输出成文件

## 一、语法

`tee [-ai] [文件...]`

## 二、参数

| 参数 | 作用 |
| :--- | :--- |
| -a | 附加到既有文件的后面，而不是覆盖它|
| -i | 忽略中断信号 |

## 三、栗子

    [rookie@izbp18hovh1qxijbodbas9z ~]$ tee test.txt
    1
    1
    2
    2
    3
    3
    ^C



