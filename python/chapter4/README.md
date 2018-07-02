#函数式编程

纯粹的函数式编程语言编写的函数式没有变量的，任意一个函数，只要输入确定，输出就是确定的。这种纯函数我们称之为是没有副作用的。

函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数。

##高阶函数

+ 变量可以指向函数
+ 函数名也是变量
+ 传入函数，变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数称之为高阶函数

###map/reduce

`map()`函数接收两个参数，一个是函数，一个是`Iterable`，`map`将传入的函数依次作用到序列的每个元素，并把结果作为新的`Iterator`返回

`reduce()`把一个函数作用到一个序列`[x1, x2, x3, ...]`上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算
    
    reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
    
`filter()`接收一个函数和一个序列，`filet()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素

`sort()`函数可以对list进行排序。还可以接收一个`key`函数来实现自定义排序。进行反向排序，不比改动key函数，传入第三个参数`reverse=True`

##返回函数

一个函数可以返回一个计算结果，也可以返回一个函数

返回一个函数时，牢记该函数并未执行，返回函数中不要引用任何可能会变化的变量

##匿名函数

    list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))

关键字`lambda`表示匿名函数，冒号前面的`x`表示函数参数

匿名函数限制，只能有一个表达式，不用写`return`，返回值就是该表达式的值

##装饰器

函数也是一个对象，函数对象可以被赋值给变量。

函数对象有一个`__name__`属性，可以拿到函数的名字。

在代码运行期间动态增加功能的方式，称之为“装饰器”（Decorator）

    def log(func):
        def wrapper(*args, **kw):
            print("call %s()" % func.__name__)
            return func(*args, **kw)
        return wrapper
        
    @log
    def now():
        print("2018-07-02")


3层嵌套的decorator

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    import functools
    
    def log(text):
        def decorator(func):
            @functools.wraps(func)
            def wrapper(*args, **kw):
                print("%s %s:" % (text, func.__name__))
                return func(*args, **kw)
            return wrapper
        return decorator
        

    @log('execute')
    def now():
        print('2018-07-02')
        
##偏函数

`functools.partial`用来创建偏函数，把一个函数的某些参数给固定住，返回一个新的函数

