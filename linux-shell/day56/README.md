# nohup命令(2019.07.29)

nohup命令可以将程序以忽略挂起的方式运行起来，被运行的程序的输出信息将不会显示到终端

## 一、语法

`nohup (选项) (参数)`

## 二、示例

使用nohup命令提交作业，如果使用nohup命令提交作业，那么在缺省情况下该作业的所有输出都被重定向到一个名为nohup.out的文件中，除非另外指定了输出文件：

```
nohup command > myout.file 2>&1 &
```

## 三、与&的区别联系

使用&后台运行程序：
- 结果会输出到终端
- 使用Ctrl + C发送SIGINT信号，程序免疫
- 关闭session发送SIGHUP信号，程序关闭

使用nohup运行程序：
- 结果默认会输出到nohup.out
- 使用Ctrl + C发送SIGINT信号，程序关闭
- 关闭session发送SIGHUP信号，程序免疫


平日线上经常使用nohup和&配合来启动程序：
同时免疫SIGINT和SIGHUP信号