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
    

##multiprocessing

`multiprocessing`模块就是跨平台版本的多进程模块，`multiprocessing`模块提供了一个`Process`类来代表一个进程对象

创建子进程时，只需要传入一个执行函数和函数的参数，创建一个`Process`实例，用`start()`方法启动，`join()`方法可以等待子进程结束后再继续往下运行，通常用于进程间的同步

    from multiprocessing import Process
    import os
    
    
    # 子进程要执行的代码
    def run_proc(name):
        print('Run child process %s (%s)...' % (name, os.getpid()))
    
    
    if __name__ == '__main__':
        print('parent process %s' % os.getpid())
        p = Process(target=run_proc, args=('test',))
        print('Child process will start')
        p.start()
        p.join()
        print('Child process end.')
        
##Pool

如果要启动大量的子进程，可以进程池方式批量创建子进程

    from multiprocessing import Pool
    import os, time, random
    
    def long_time_task(name):
        print('Run task %s (%s)...' % (name, os.getpid()))
        start = time.time()
        time.sleep(random.random() * 3)
        end = time.time()
        print('Task %s runs %0.2f sends.' % (name, (end - start)))
    
    if __name__ == '__main__':
        print('Parent process %s.' % os.getpid())
        p = Pool(4)
        for i in range(5):
            p.apply_async(long_time_task, args=(i,))
        print('Waiting for all subprocess done...')
        p.close()
        p.join()
        print('All subprocess done.')
        

对`Pool`对象调用`join()`方法会等待所有的子进程执行完毕，调用`join()`之前必须先调用`close()`方法，调用`close()`之后就不能添加新的`Process`了

##子进程

`subprocess`模块可以让我们非常方便的启动一个子进程，然后控制其输入和输出

    import subprocess
    
    print('$ nslookup www.python.org')
    r = subprocess.call(['nslookup', 'www.python.org'])
    print('Exit code:', r)

##进程间通信

`Process`之间肯定是需要通信的，Python的`multiprocessing`模块封装了底层的机制，提供了`Queue`、`Pipes`等多种方式进行交换数据

    import os
    import random
    import time
    from multiprocessing import Process, Queue
    
    
    def write(q):
        print('Process to write:%s' % os.getpid())
        for value in ['A', 'B', 'C']:
            print('put %s to queue...' % value)
            q.put(value)
            time.sleep(random.random())
    
    
    def read(q):
        print('Process to read:%s' % os.getpid())
        while True:
            value = q.get(True)
            print('Get %s from queue.' % value)
    
    
    if __name__ == '__main__':
        q = Queue()
        pw = Process(target=write, args=(q,))
        pr = Process(target=read, args=(q,))
    
        pw.start()
        pr.start()
        pw.join()
        pr.terminate()
        
#多线程

Python标准库提供了两个模块：`_thread`和`threading`，`_thread`是低级模块，`threading`是高级模块，对`_thread`进行了封装。

启动一个线程就是把一个函数传入并创建`Thread`实例，然后调用`start()`开始执行

    import threading
    import time
    
    
    def loop():
        print('thread %s is running...' % threading.current_thread().name)
        n = 0
        while n < 5:
            n = n + 1
            print('thread %s >>> %s' % (threading.current_thread().name, n))
            time.sleep(1)
        print('thread %s ended' % threading.current_thread().name)
    
    
    print('thread %s is running...' % threading.current_thread().name)
    t = threading.Thread(target=loop, name='LoopThread')
    t.start()
    t.join()
    print('thread %s ended.' % threading.current_thread().name)
    
###Lock

Python中使用`lock.acquire()`加锁，防止多个线程同时操作共享变量

    import threading
    
    balance = 0
    lock = threading.Lock()
    
    
    def change_it(n):
        global balance
        balance = balance + n
        balance = balance - n
    
    
    def run_thread(n):
        for i in range(10000):
            lock.acquire()
            try:
                change_it(n)
            finally:
                lock.release()
    
    
    t1 = threading.Thread(target=run_thread, args=(5,))
    t2 = threading.Thread(target=run_thread, args=(8,))
    t1.start()
    t2.start()
    t1.join()
    t2.join()
    print(balance)
    
#ThreadLocal

全局变量`local_school`就是一个`ThreadLocal`对象，每个`Thread`对它都可以读写`student`属性，但互不影响

`ThreadLocal`最常用的地方就是为每个线程绑定一个数据库连接、HTTP请求、用户身份信息等

    import threading
    
    local_school = threading.local()
    
    
    def process_student():
        std = local_school.student
        print('Hello, %s (in %s)' % (std, threading.current_thread().name))
    
    
    def process_thread(name):
        local_school.student = name
        process_student()
    
    
    t1 = threading.Thread(target=process_thread, args=('rookie',), name='Thread-A')
    t2 = threading.Thread(target=process_thread, args=('wdn',), name='Thread-B')
    t1.start()
    t2.start()
    t1.join()
    t2.join()
    
#分布式进程

//TODO:

