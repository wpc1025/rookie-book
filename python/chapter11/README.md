#正则表达式

常用字符表示：

+ 直接给出字符就是精确匹配
+ `\d`表示一个数字
+ `\w`可以匹配一个字母或数字
+ `.`可以匹配任意字符
+ `\s`可以匹配一个空格（也包括Tab等空白符）
+ `*`表示任意个字符（包括0个），`+`表示至少一个字符，`?`表示0个或1个字符，`{n}`表示n个字符，`{n,m}`表示n-m个字符
+ `[]`表示范围，`A|B`可以匹配A或B，`^`表示行的开头，`$`表示行的结束

Python中，提供了`re`模块，包含所有正则表达式的功能。`match()`方法判断是否匹配，匹配成功，返回一个`Match`对象，否则返回`None`

    import re
    
    result = re.match(r'^\d{3}-\d{3,8}$', '010-12345')
    print(result)
    
    result = re.match(r'^\d{3}-\d{3,8}$', '13500827560')
    print(result)
    
##切分字符串

用正则表达式切分字符串比用固定字符串更灵活

    import re
    print('a b  c    d'.split(' '))
    print(re.split(r'\s+', 'a b  c    d'))
    print(re.split(r'[\s\,]+', 'a b  c ,    d'))
    
##分组

正则表达式还有提取子串的强大功能，用`()`表示的就是要提取的分组

    import re
    result = re.match(r'^(\d{3})-(\d{3,8})$', '010-12345')
    print(result.group(0))
    print(result.group(1))
    print(result.group(2))
    
##贪婪匹配

正则匹配默认是贪婪匹配，也就是匹配尽可能多的字符。举例如下，匹配出数字后面的0：

    >>> re.match(r'^(\d+)(0*)$', '102300').groups()
    ('102300', '')
    
由于`\d+`采用贪婪匹配，直接把后面的0全部匹配了，结果`0*`只能匹配空字符串了。

必须让`\d+`采用非贪婪匹配（也就是尽可能少匹配），才能把后面的0匹配出来，加个`?`就可以让`\d+`采用非贪婪匹配：

    >>> re.match(r'^(\d+?)(0*)$', '102300').groups()
    ('1023', '00')
    
##编译

若一个正则表达式使用多次，我们可以采用预编译的形式

    import re
    re_telphone = re.compile(r'^(\d{3})-(\d{3,8})$')
    print(re_telphone.match('010-12345').groups())
    print(re_telphone.match('010-8086').groups())