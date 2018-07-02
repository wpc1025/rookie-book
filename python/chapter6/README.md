#面向对象编程

+ 类是创建实例的模板，而实例则是一个一个具体的对象，各个实例拥有的数据都互相独立，互不影响；
+ 方法就是与实例绑定的函数，和普通函数不同，方法可以直接访问实例的数据；
+ 通过在实例上调用方法，我们就直接操作了对象内部的数据，但无需知道方法内部的实现细节。
+ 和静态语言不同，Python允许对实例变量绑定任何数据，也就是说，对于两个实例变量，虽然它们都是同一个类的不同实例，但拥有的变量名称都可能不同

类的定义：

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    class Student(object):
    
    
        def __init__(self, name, score):
            self.name = name
            self.score = score
    
        def print_score(self):
            print('%s : %s' % (self.name, self.score))
            
##访问限制

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-、
    
    class Student(object):
        
        def __init__(self, name, score):
            self.__name = name
            self.__score = score
    
        def print_score(self):
            print('%s : %s' % (self.__name, self.__score))

+ 如果要让类内部属性不被外部访问，可以把属性的名称前加上两个下划线`__`，在Python中，实例的变量名如果以`__`开头，就变成了一个私有变量，只有内部可以访问，外部不能访问
+ 如果外部需要修改或访问类内的私有变量，可以通过设置set或get方法
+ 私有变量其实是可以通过`实例名._类名__属性`来访问的，如`rookie_Student__name`

##继承和多态

##获取对象信息

+ `type()`函数返回变量对应的Class类型。判断基本数据类型可以直接使用`int`、`str`等，判断一个对象是否是函数，可以用`types`模块中定义的常量
+ `isinstance()`判断一个对象是否是类型本身，或者位于该类型的父继承链上
+ 能用`type()`判断的类型也可以用`isinstance()`判断
+ `isinstance()`还可以判断一个变量是否是某些类型中的一种
+ `dir()`可以获得一个对象的所有属性和方法，list类型。配合`getattr()`、`setattr()`以及`hasattr()`，可以直接操作一个对象的状态


##实用属性和类属性

+ 给实例绑定对象，是通过实例变量或者`self`变量
+ 在class中定义的属性，是类属性
+ 相同名称的实例属性会屏蔽掉类属性

