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

