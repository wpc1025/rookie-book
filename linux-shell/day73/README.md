# cal(2019.08.15)

用来显示公历（阳历）日历

## 一、语法

`cal [参数] [月份] [年份]`

## 二、参数

| 参数 | 作用 |
| :--- | :--- |
| -1 | 显示一个月的月历 |
| -3 | 显示系统前一个月，当前月，下一个月的月历 |
| -s | 显示星期天为一个星期的第一天，默认的格式 |
| -m | 显示星期一为一个星期的第一天 |
| -j | 显示在当年中的第几天 |
| -y | 显示当前年份的日历 |

## 三、实例

### 3.1 显示当前月份日历

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ cal
     August 2019    
Su Mo Tu We Th Fr Sa
             1  2  3
 4  5  6  7  8  9 10
11 12 13 14 15 16 17
18 19 20 21 22 23 24
25 26 27 28 29 30 31

```

### 3.2 显示指定月份的日历

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ cal 8 2019
     August 2019    
Su Mo Tu We Th Fr Sa
             1  2  3
 4  5  6  7  8  9 10
11 12 13 14 15 16 17
18 19 20 21 22 23 24
25 26 27 28 29 30 31

```

### 3.3 显示指定年的日历

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ cal -y 2019
```

### 3.4 天数

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ cal -j
        August 2019        
Sun Mon Tue Wed Thu Fri Sat
                213 214 215
216 217 218 219 220 221 222
223 224 225 226 227 228 229
230 231 232 233 234 235 236
237 238 239 240 241 242 243
```

参考博客地址：

[linux命令大全之cal命令详解(显示日历)](https://blog.csdn.net/u010071211/article/details/80224265)
