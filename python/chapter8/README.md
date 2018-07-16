#错误、调试和测试

##错误处理

###try

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    try:
        print('try...')
        r = 10 / 0
        print('result',r)
    except ValueError as e:
        print('valueError',e)
    except ZeroDivisionError as e:
        print('zeroDivisionError',e)
    else:
        print('no error')
    finally:
        print('finally...')
    print('END')
    
+ 当我们认为某些代码可能会出错时，就可以用`try`来运行这段代码，如果执行出错，则后续代码不会继续执行，而是直接跳转至错误处理代码，即`except`语句块，执行完`except`后，如果有`finally`语句块，则执行`finally`语句块，至此，执行完毕
+ 如果没有错误发生，可以在`except`语句块后面加一个`else`，当没有错误发生时，会自动执行`else`语句
+ Python的错误也是class，所有的错误类型都继承自`BaseException`，捕获到父类异常之后，是不会执行子类异常的
+ 常见错误类型和错误关系，可以查看[这里](https://docs.python.org/3/library/exceptions.html#exception-hierarchy)

##调用栈

##记录错误

Python内置的`logging`模块可以非常容易的记录错误信息。通过配置，`logging`还可以把错误信息记录到日志文件中。

##抛出错误

#调试

+ 可以在代码中使用`print()`函数打印出信息去调试（不推荐）
+ 可以使用断言`assert`语句代替`print()`，启动Python解释器时可以用`-0`参数来关闭`assert`
+ 使用`logging`
+ 以参数`-m pdb`启动，使程序单步执行
+ 使用`pdb.set_trace()`设置断点

#单元测试

+ 单元测试是用来对一个模块、一个函数或者一个类来进行正确性检查的测试工作
+ 引入Python自带的`unittest`模块，编写单元测试
+ 测试类从`unittest.TestCase`继承
+ 通过参数`-m unittest`直接运行单元测试
+ `setUp()`和`tearDown()`方法在调用每一个测试方法的前后被执行

#文档测试

Python内置的`doctest`模块可以直接提取注释中的代码并执行测试


