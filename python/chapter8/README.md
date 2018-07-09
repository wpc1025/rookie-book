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

