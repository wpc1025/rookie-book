#Python基础
##输入输出

###输出

`print('hello, world.')`  
`print('i','am','rookie')`

###输入

`name = input()`  
`name = input('please enter your name:')`  

##数据类型和变量

###整数

Python可以处理任意大小正负整数

###浮点数

整数和浮点数在计算机内部存储方式不同，整数运算永远是精确地，浮点数运算则有可能会有四舍五入的误差

Python的浮点数没有大小限制，但是超出一定范围就直接表示为`inf`无限大

###字符串

+ 字符串是以单引号`'`或双引号`"`括起来的任意文本
+ 支持转义字符，如`\n`表示换行，`\t`表示制表符等
+ 支持`'''...'''`表示多行内容

###布尔值

+ 一个布尔值只有`True`、`False`两种值
+ `and`运算是与运算，`or`运算是或运算，`not`运算是非运算

###空值

空值是Python里一个特殊的值，用`None`表示

###变量

变量名必须是大小写英文、数字和`_`的组合，且不能用数字开头

###常量

常量就是不能变的变量，Python中，通常用全部大写的变量名表示常量

+ `/`除法计算结果是浮点数，即使是两个证书恰好整除，结果也是浮点数
+ `//`地板除，两个整数的相除任然是整数
+ `%`取余

##字符串和编码

+ Python3中字符串以Unicode编码，Python的字符串支持多语言
+ `ord()`函数获取字符的整数表示，`chr()`函数把编码转换成对应的字符
+ `len()`函数计算字符串的字节数
+ 源代码文件头部增加`# -*- coding: utf-8 -*-`，以告知Python解释器按照UTF-8读取源代码
+ `%`运算符可用来格式化字符串。`%s`代表字符串，`%d`代表整数，`%f`代表浮点数，`%x`代表十六进制整数。用法例如：`'Hello, %s' % 'world'`
+ 对`%`进行转义使用`%%`
+ 也可使用`format()`函数格式化字符串

##使用list和tuple

###list

+ 列表，有序集合，随时添加和删除其中的元素
+ `classmates = ['wpc','zhh','zh']`定义一个list集合
+ `len(classmates)`可以获得list元素的个数
+ 用索引来访问list中每一个位置的元素，索引从0开始，例如`classmates[0]`
+ 如果要取最后一个元素，   索引使用`-1`即可，如`classmates[-1]`
+ list是一个可变的有序列表，可以通过`append()`来追加元素，如`classmates.append('wy')`
+ 可插入元素到指定位置，如`classmates.insert(1,'wyh')`
+ 删除list末尾的元素，用`pop()`方法，要删除指定位置的元素，使用`pop(i)`方法，`i`是索引位置
+ list中元素的数据类型可以不同
+ list中也可以包含另一个list，如：`classmates = ['wpc','wy','zhh',['wyh','glc'],'ype']`

###tuple

+ 称之为元组，有序列表，一旦初始化就不可修改，所谓不可修改（不变），指的是tuple中的每个元素，指向永远不变
+ 当`tuple`定义一个元素时，必须使用`t = (1,)`方式，以区分数学公式中的小括号

###条件判断
    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    age = 3
    print('your age is',age)
    if age >= 18:
        print('adult')
    elif age >= 6:
        print('teenager')
    else:
        print('kid')
        
+ `if`语句从上往下判断，若某个判断是`True`，就忽略剩下的`elif`和`else`
+ 非零数值、非空字符串、非空list等，都判断为   `True`，否则为`False`
+ `int()`函数可以将`str`类型转换为整数

###循环

+ `for x in ...`循环，`range(x)`可以生成一个0-x的序列，通过`list(range(x))`可以把`range(x)`生成的序列转换成list

        #!/usr/bin/env python3
        # -*- coding: utf-8 -*-
        sum = 0
        for x in range(101):
        sum = sum + x
        print(sum)

+ `while`循环

        #!/usr/bin/env python3
        # -*- coding: utf-8 -*-
        sum = 0
        n = 99
        while n > 0:
            sum = sum + n
            n = n - 2
        print(sum)
        
+ `break`可以提前跳出循环
+ `continue`跳过当前勋循环，直接开始下一次循环

##使用dict和set

###dict

+ 称之为字典，类似于java中的Map，使用键值对存储，具有极快查找速度
+ dict的定义，实例如下：
        
        d = {'wpc':100,'zhh':100,'wy':101}

+ 从dict中获取一个不存在的key时，会报错。判断key是否存在，两种方式：

        'wpc' in d
        d.get('wpc',-1)

+ 从dict中删除一个key，用`pop(key)`方法
+ dict的key必须不可变，字符串和整数不可变，list可变

###set

+ 一组key的集合，key不重复，无序
+ 定义方式如下：

        s = set([1, 2, 3])
        
+ `add(key)`添加元素到set中，`remove(key)`删除元素

###不可变对象









