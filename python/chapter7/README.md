#面向对象高级编程

##使用__slots__

+ `__slots__`变量限制class实例能添加的属性
+ `__slots__`定义的属性仅对当前类实例起作用，对继承的子类是不起作用的。除非在子类中也定义`__slots__`，子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__`

##使用@property

Python内置的`@property`装饰器负责把一个方法变成属性调用

+ 把一个getter方法变成属性，只需要加上`@property`就可以了，`@score.setter`负责把一个setter方法变成属性赋值
+ 只定义getter方法，不定义setter方法就是一个只读属性

##多重继承

##定制类

+ `__str__()`返回用户看到的字符串，`__repr__()`返回程序开发者看到的字符串
+ 如果一个类想被用于`for ... in`循环，类似于`list`或`tuple`那样，就必须实现一个`__iter__()`方法，该方法返回一个迭代对象。然后，Python的for循环就会不断调用该迭代对象的`__next__()`方法拿到循环的下一个值，直到遇到StopIteration错误时退出循环。
+ `__getitem__()`方法设置类像list那样按照下标取出元素
+ `__getattr__()`
+ `__call__()`

##使用枚举类

`Enum`可以把一组相关常量定义在一个class中，且class不可变，而且成员可以直接比较。

##使用元类

`type()`函数可以返回一个对象的类型，也可以创建新的类型，如下：

    def fn(self, name='world'): # 先定义函数
        print('Hello, %s.' % name)
        
    Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class
    h.hello()
    print(type(Hello))
    print(type(h))
     
要创建一个class对象，`type()`函数依次传入3个参数：
1. class的名称
2. 继承的父类集合
3. class的方法名称与函数绑定

###metaclass

+ `metaclass`，直译为元类
+ 先定义`metaclass`，就可以创建类，最后创建实例



