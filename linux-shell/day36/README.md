# nl命令(2019.07.09)

编号过滤工具，计算输入中的行号，将计算过的行号写入标准输出。

## 一、语法格式

`nl [参数] [文件]`

## 二、常用参数

| 参数 | 作用 |
| :--- | :--- |
| -b | 指定行号指定的方式 |
| -n | 列出行号表示的方式 |
| -w | 行号栏位的占用的位数 |
| -p | 在逻辑定界符处不重新开始计算 |

## 三、栗子

### 3.1 用nl列出文件的内容

    [rookie@izbp18hovh1qxijbodbas9z wechat-spring-boot]$ nl rookie.log 
    
### 3.2 用nl列出文件的内容，空白行也加上行号

    [rookie@izbp18hovh1qxijbodbas9z wechat-spring-boot]$ nl -b a rookie.log 