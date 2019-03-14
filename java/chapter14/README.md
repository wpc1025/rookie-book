# 第十四章 并发

多任务：在同一刻运行多个程序的能力。  

多进程与多线程的区别：
+ 多进程与多线程的本质区别在于每个进程拥有自己的一整套变量，而线程则共享数据。
+ 共享变量使线程之间的通信比进程之间的通信更有效、更容易。
+ 与进程相比较，线程更"轻量级"，创建、撤销一个线程比启动新进程的开销要小得多。

## 14.1 什么是线程


