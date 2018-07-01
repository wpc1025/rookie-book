#高级特性

##切片

Python提供了切片操作符简化经常取指定索引范围的操作

     L = list(range(100))
    
    #取前三个元素
    L[0:3]
    L[:3]
    
    #取前十个数
    L[:10]
    
    #取后十个数
    L[-10:]
    
    #前11-20个数
    L[10:20]
    
    #前10个数，每两个取一个
    L[:10:2]
    
    #所有数，每五个取一个
    L[::5]
    
    #原样复制一个list
    L[:]
    

分片对于`tuple`与`str`同样适用

##迭代

`for ... in`可以用来迭代Python中任何可迭代的对象，如`list`，`tuple`，`dict`

通过`collections`模块的`Iterable`类型可以判断一个对象是否是可迭代对象，如

    from collections import Iterable
    isinstance('abc',Iterable)
    
Python内置的`enumerate`函数可以将一个`list`变成索引-元素对，例如：

    for i,value in enumerate([1,2,3]):
        print(i,value)
        
##列表生成式

    #普通生成式
    [x * x for x in range(1,11)]
    #列表生成式使用判断
    [x * x for x in range(1,11) if x%2  == 0]
    #使用两层循环
    [m + n for m in 'ABC' for n in 'XYZ']
    
##生成器

生成器：一边循环一边计算的机制

只需要把列表生成的`[]`改成`()`，就创建了一个生成器

    g = ( x * x for x in range(10))
    
循环调用显示生成器中的每一个元素：

    for n in g:
        print(n)
        
如果一个函数定义中包含了`yield`关键字，那么这个函数就不再是一个普通函数，而是一个生成器：

##迭代器

可以被`next()`函数调用并不断返回下一个值的对象称之为迭代器：`Iterator`

生成器都是`Iterator`对象，但`list`、`dict`、`str`虽然是`Iterable`，却不是`Iterator`

把`list`、`dict`、`str`等Iterable变成`Iterator`可以使用`iter()`函数

    
    
    