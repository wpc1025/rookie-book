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
+ Python函数返回多个值，其实返回值是一个tuple，返回一个tuple可以省略括号，而多个变量可以接收一个tuple，按位置赋予对应的值

##函数的参数

函数除正常定义的**必选参数**，还可以使用**默认参数**、**可变参数**、**关键字参数**

###位置参数

###默认参数

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    def power(x,n=2):
        s = 1
        while n > 0:
            n = n - 1
            s = s * x
    
        return s

注意事项
+ 必选参数在前，默认参数在后
+ 当函数有多个参数时，把变化大的参数放前面，变化小的参数放在后面。变化小的参数可以作为默认参数
+ 当有多个默认参数时，不按顺序提供默认参数时，需要把参数名加上
+ 默认参数必须指向不变对象。

###可变参数

定义可变参数函数

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    def calc(*numbers):
        sum = 0
        for n in numbers:
            sum = sum + n * n
        return sum

调用可变参数

    #正常调用
    calc(1,2,3,4)
    
    #空调用
    calc()
    
    #list调用
    nums = [1,2,3,4]
    calc(*nums)

可变参数接收到的是一个`tuple`

###关键字参数

关键字参数允许传入零个或任意个含参数名的参数，这些关键字参数在函数内部自动封装成一个`dict`

关键字参数函数定义：

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    def person(name,age,**kw):
        print('name',name,'age',age,'other',kw)
        
关键字参数函数调用

    person('rookie',20)

    person('rookie',20,city='xian')
    
    person('rookie',20,city='xian',job='gcs')
    
    extra={'city':'xian','job':'gcs'}
    person('rookie',20,**extra)
    
###命名关键字函数

限制关键字参数的名字，就可以使用命名关键字

只接受`city`和`job`作为关键字参数，定义如下：

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    def person(name,age,*,city,job):
        print(name,age,city,job)
        

调用方式如下：

    person('rookie',20,city='xian',job='gcs')
    
如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不需要一个特殊分隔符`*`了，例如：

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    
    def person(name,age,*args,city,job):
        print(name,age,args,city,job)

命名关键字函数可以有缺省值，例如：

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    def person(name,age,*,city='BeiJing',job):
        print(name,age,city,job)

###参数组合

Python中定义函数，参数顺序必须满足：必选参数、默认参数、可变参数、命名关键字参数、关键字参数


