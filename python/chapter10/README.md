#进程和线程

线程是最小的执行单元，而进程至少有一个线程组成

多进程和多线程的程序涉及到同步、数据共享的问题

##多进程

Unix/Linux操作系统提供了一个`fork()`系统调用。`fork()`调用一次，返回两次，子进程永远返回0，父进程返回子进程的ID。Python的`os`模块封装了常见的系统调用，其中就包括`fork`

    #!/usr/bin/env python3
    # -*- coding:utf-8 -*-
    
    # os.fork()只在Unix/Linux系统中才可以使用
    
    import os
    import time
    
    print('process (%s) start' % os.getpid())
    
    pid = os.fork()
    
    if pid == 0:
        print('I am child process (%s) and my parent is %s.' % (os.getpid(), os.getppid()))
    else:
        print('I (%s) just created a child process (%s).' % (os.getpid(), pid))
    
    time.sleep(1000)
    
