# watch命令(2019.06.26)

- 监测一个命令的运行结果
- 默认以2s的间隔重复运行命令，使用`-n`参数指定时间间隔
- 使用`-d`高亮显示变化的区域

## 一、命令格式

`watch [options] command`

## 二、命令参数

| 参数 | 作用 |
| -d | 高亮显示变动 |
| -n | 周期 |
| -t | 会关闭watch命令在顶部的时间间隔 |

## 三、栗子

### 3.1 监控文件内容的变化

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ watch -n 2 -d cat test.txt 
    Every 2.0s: cat test.txt                                                                                               Tue Jun 25 22:25:52 2019
    
    11 aa
    33 cc
    23 dd
    55 2e
    llllllllllll


