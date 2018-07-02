#模块

+ 在Python中，一个.py文件就称之为一个模块
+ 为避免模块名冲突，引入按目录来组织模块的方法，称为包


##使用模块

编写模块：

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    ' a test module '
    
    __author__ = 'Michael Liao'
    
    import sys
    
    def test():
        args = sys.argv
        if len(args)==1:
            print('Hello, world!')
        elif len(args)==2:
            print('Hello, %s!' % args[1])
        else:
            print('Too many arguments!')
    
    if __name__=='__main__':
        test()

注意这两行代码

    if __name__=='__main__':
        test()

在命令行运行该模块时，Python解释器把一个特殊变量`__name__`置为`__main__`，如果在其它地方导入该模块时，`if`判断将失败，因此，这种`if`测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试。

###作用域

在Python中，通过`_`前缀来实现限制模块内某些变量或函数不被外部调用

类似`__xxx__`可以被直接引用

##安装第三方模块

在Python中，安装第三方模块，是通过包管理工具pip完成的

例如，安装`Pillow`命令如下：

    pip install Pillow
    

###安装常用模块

使用`pip`一个一个安装模块，费时费力，推荐直接使用`Anaconda`，是一个基于Python的数据处理和科学计算平台，它已经内置了许多非常有用的第三方库。

###模块搜索路径

默认情况，Python会搜索当前目录、所有已安装的内置模块和第三方模块

添加搜索目录，有两种方法：
+ 直接修改`sys.path`
+ 设置环境变量`PYTHONPATH`

