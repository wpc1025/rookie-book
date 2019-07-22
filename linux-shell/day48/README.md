# du命令(2019.07.21)

用于显示目录或文件的大小

## 一、语法

`du [选项] [文件]`

## 二、参数

| 参数 | 作用 |
| :--- | :--- |
| -a | 显示目录中个别文件的大小 |
| -b | 显示目录或文件大小时，以byte为单位 |
| -c | 除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和 |
| -k | 以kb为单位输出 |
| -m | 以MB为单位输出 |
| -s | 仅显示总计，只列出最后加总的值 |
| -h | 以K、M、G为单位，提高信息的可读性 |
| -x | 以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过 |
| -L | 显示选项中所指定符号链接的源文件大小 |
| -S | 显示个别目录的大小时，并不含其子目录的大小 |
| -X | 指定目录或文件 |
| --exclude | 略过指定的目录或文件 |
| -D | 显示指定符号链接的源文件大小 |
| -H | 与-h参数相同，但是K、M、G是以1000为换算单位 |

## 三、实例

### 3.1 方便阅读的格式显示

```
[rookie@izbp18hovh1qxijbodbas9z wechat-spring-boot]$ du -ah
39M	./wechat-spring-boot.jar
320K	./logs.tar
12K	./logs/rookie.log.2019-03-06
4.0K	./logs/rookie_log.tar.bz2
12K	./logs/rookie.log.2019-03-20
12K	./logs/rookie.log.2019-03-18
12K	./logs/rookie.log.2019-03-08
12K	./logs/rookie.log.2019-03-04
12K	./logs/rookie.log.2019-03-02
12K	./logs/rookie.log.2019-03-27
12K	./logs/rookie.log.2019-03-05
12K	./logs/rookie.log.2019-03-01
12K	./logs/rookie.log.2019-03-14
12K	./logs/rookie.log.2019-02-26
12K	./logs/rookie.log.2019-03-22
12K	./logs/rookie.log.2019-03-10
12K	./logs/rookie.log.2019-03-07
12K	./logs/rookie.log.2019-03-24
4.0K	./logs/rookie.log
12K	./logs/rookie.log.2019-03-19
12K	./logs/rookie.log.2019-03-13
12K	./logs/rookie.log.2019-03-21
12K	./logs/rookie.log.2019-02-28
12K	./logs/rookie.log.2019-03-15
8.0K	./logs/rookie.log.2019-03-25
12K	./logs/rookie.log.2019-03-12
12K	./logs/rookie.log.2019-03-16
12K	./logs/rookie.log.2019-02-27
12K	./logs/rookie.tar
12K	./logs/rookie.log.2019-03-26
12K	./logs/rookie.log.2019-03-09
12K	./logs/rookie.log.2019-03-03
12K	./logs/rookie.log.2019-03-23
12K	./logs/rookie.log.2019-03-11
12K	./logs/rookie.log.2019-03-17
4.0K	./logs/rookie_log.tar.gz
384K	./logs
4.0K	./rookie.log
39M	.
[rookie@izbp18hovh1qxijbodbas9z wechat-spring-boot]$
```




