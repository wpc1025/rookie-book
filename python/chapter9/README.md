#IO编程

##文件读写

###读文件
+ 使用`open()`函数打开一个文件对象，传入文件名和标识符 
+ 若文件不存在，会抛出错误`IOError`错误
+ Python引入`with`语句来自动帮助我们调用`close()`方法
+ 如果文件很小，`read()`一次性读取很方便，如果不能确定文件大小，反复调用`read(size)`比较保险，如果是配置文件，调用`readlines()`最方便


    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    
    def openFile1(filePath):
        try:
            f = open(filePath,'r',encoding='utf-8')
            print(f.read())
        finally:
            if f:
                f.close()
    
    def openFile2(filePath):
        with open(filePath,'r',encoding='utf-8') as f:
            print(f.read())
    
    def openFile3(filePath):
        with open(filePath,'r',encoding='utf-8') as f:
            str = f.read(1024)
            while str:
                print(str)
                str = f.read(1024)
    
    def openFile4(filePath):
        with open(filePath,'r',encoding='utf-8') as f:
            for line in f.readlines():
                print(line.strip())
    
    filePath = 'C:\\Users\\rookie\\Desktop\\记录.txt'
    
    # openFile1(filePath)
    
    # openFile2(filePath)
    
    # openFile3(filePath)
    
    openFile4(filePath)

###file-like Object

像`open()`函数返回的这种有个`read()`方法的对象，在Python中统称为file-like Object

###二进制对象

要读取二进制对象，比如图片、视频等，用`rb`模式打开文件即可

    with open(file_path, 'rb') as f:
        print(f.read())
        
###字符编码

+ 要读取非UTF-8编码的文本文件，需要给`open()`函数传入`encoding`参数
+ 遇到编码不规范的文件，会遇到`UnicodeDecodeError`，这种情况，`open()`函数接收一个`errors`参数，表示遇到编码错误后如何处理，最简单的方式是忽略


    f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')
    
###写文件

写文件调用`open()`函数传入模式`w`或者`wb`标识写文本文件还是二进制文件

传入模式`a`以追加模式写入

    def write_append_file(file_path):
        with open(file_path, 'a', encoding='utf-8') as f:
            f.write("Hello Python")
            
    def write_file(file_path):
        with open(file_path, 'w', encoding='utf-8') as f:
            f.write('Hello rookie')
            
###StringIO

内存中读写

    from io import StringIO
    
    f = StringIO()
    f.write('hello')
    f.write(' ')
    f.write('Python')
    
    print(f.getvalue())
    
    f = StringIO('Hello!\nHi!\nGoodbye!')
    while True:
        s = f.readline()
        if s == '':
            break
        print(s.strip())
        
###BytesIO

内存中读写

    from io import BytesIO
    
    f = BytesIO()
    f.write('王大鸟'.encode('utf-8'))
    print(f.getvalue())
    
    f = BytesIO('王大鸟'.encode('utf-8'))
    print(f.read())
    
###操作文件和目录

Python的`os`模块封装了操作系统的目录和文件系统，这些函数有的在`os`中，有的在`os.path`中

    import os
    
    # 获取操作系统类型
    print(os.name)
    # 获取详细的操作系统信息(winows无法使用）
    # print(os.uname())
    # 获取环境变量
    print(os.environ)
    print(os.environ.get('PATH'))
    
    # 查看当前目录的绝对路径
    print(os.path.abspath('.'))
    # 在某个目录下创建一个新目录，首先把目录的完整路径表示出来
    os.mkdir('C:\\Users\\rookie\\Desktop\\pythonMkdir')
    os.rmdir('C:\\Users\\rookie\\Desktop\\pythonMkdir')
    
    # 得到最后一级文件名或者文件夹名
    print(os.path.split('C:\\Users\\rookie\\Desktop\\pythonMkdir'))
    
    print(os.path.splitext('C:\\Users\\rookie\\Desktop\\记录.txt'))
    
    # 对文件重命名
    # os.rename('test.txt', 'test.py')
    # 删除文件
    # os.remove('test.py')
    
    # 列出当前文件夹下的目录
    print([x for x in os.listdir('.') if os.path.isdir(x)])
    # 列出当前文件夹下的python文件
    print([x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1] == '.py'])

###序列化

我们把变量从内存中变成可存储或传输的过程称之为序列化，反过来，把变量内容从序列化的对象重新读取到内存里称之为反序列化



