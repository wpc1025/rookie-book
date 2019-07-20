# export命令(2019.07.20)

export命令用于将shell变量输出为环境变量，或者将shell函数输出位环境变量

## 一、语法

`export (选项) (参数)`

## 二、选项

| 选项 | 作用 |
| :--- | :--- |
| -f | 代表变量名称为函数名称 |
| -n | 删除指定的变量。变量实际上并未删除，只是不会输出到后序指令的执行环境中 |
| -p | 列出所有shell赋予程序的环境变量 |

## 二、参数

变量： 指定要输出或者删除的环境变量

## 三、实例

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ export rookie=wpc
[rookie@izbp18hovh1qxijbodbas9z ~]$ echo $rookie
wpc
```