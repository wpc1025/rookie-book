#函数

##调用函数

python内置了很多有用的函数，可以通过[官方文档](https://docs.python.org/3/library/functions.html)查看

##定义函数

+ 定义函数使用`def`语句，依次写出函数名、括号、括号中的参数和冒号`:`，在缩进块中编写函数体，函数的返回值用`return`返回。如：
        #!/usr/bin/env python3
        # -*- coding: utf-8 -*-
        
        def my_abs(x):
            if x >= 0:
                return x
            else:
                return -x

+ 空函数，如果想定义一个什么也不做的空函数，可以使用`pass`语句，相当于java中的`TODO`，如：
        
        def nop():
            pass            

+ 参数检查，数据类型检查可以使用参数`isinstance()`实现
+ Python函数返回多个值


            