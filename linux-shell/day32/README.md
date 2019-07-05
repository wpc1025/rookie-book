# xargs命令(2019.07.05)

- linux命令可以从两个地方读取要处理的内容，一个是通过命令行参数，一个是标准输入
- `xargs`可以通过管道符接受字符串，并将接收到的字符串通过空格分割成许多参数（默认情况下通过空格分隔），然后将参数传递给后面的命令，作为命令的命令行参数

## 一、 xargs 与 管道的不同

    [rookie@izbp18hovh1qxijbodbas9z ~]$ echo '--help' | cat
    --help
    [rookie@izbp18hovh1qxijbodbas9z ~]$ echo '--help' | xargs cat
    Usage: cat [OPTION]... [FILE]...
    Concatenate FILE(s), or standard input, to standard output.
    
      -A, --show-all           equivalent to -vET
      -b, --number-nonblank    number nonempty output lines, overrides -n
      -e                       equivalent to -vE
      -E, --show-ends          display $ at end of each line
      -n, --number             number all output lines
      -s, --squeeze-blank      suppress repeated empty output lines
      -t                       equivalent to -vT
      -T, --show-tabs          display TAB characters as ^I
      -u                       (ignored)
      -v, --show-nonprinting   use ^ and M- notation, except for LFD and TAB
          --help     display this help and exit
          --version  output version information and exit
    
    With no FILE, or when FILE is -, read standard input.
    
    Examples:
      cat f - g  Output f's contents, then standard input, then g's contents.
      cat        Copy standard input to standard output.
    
    GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
    For complete documentation, run: info coreutils 'cat invocation'
    [rookie@izbp18hovh1qxijbodbas9z ~]$ 

## 二、选项

### 3.1 -d 选项

使用`-d`命令指定分隔符

    [rookie@izbp18hovh1qxijbodbas9z ~]$ echo '11@22@33' | xargs echo 
    11@22@33
    [rookie@izbp18hovh1qxijbodbas9z ~]$ echo '11@22@33' | xargs -d @ echo 
    11 22 33
    
    [rookie@izbp18hovh1qxijbodbas9z ~]$
    
### 3.2 -p 选项

使用该选项之后，`xargs`并不会马上执行其后面的命令，而是输出即将要执行的完整的命令（包括命令以及传递给命令的命令行参数），询问是否执行

    [rookie@izbp18hovh1qxijbodbas9z ~]$ echo '11@22@33' | xargs -p -d @ echo 
    echo 11 22 33
     ?...y
    11 22 33
    
    [rookie@izbp18hovh1qxijbodbas9z ~]$ 
    
### 3.3 -n 选项

该选项表示将`xargs`生成的命令行参数，每次传递几个参数给其后面的命令执行

    [rookie@izbp18hovh1qxijbodbas9z ~]$ echo '11@22@33@44@55@66@77@88@99' | xargs -d @ -n 3 echo 
    11 22 33
    44 55 66
    77 88 99
    
    [rookie@izbp18hovh1qxijbodbas9z ~]$
    
### 3.4 -E 选项

该选项指定一个字符串，当`xargs`解析出多个命令行参数的时候，如果搜到`-E`指定的命令行参数，则只会将`-E`指定的命令行参数之前的参数（不包括`-E`指定的这个参数）传递给`xargs`后面的命令

`-E`只有在`xargs`不指定`-d`的时候有效

    [rookie@izbp18hovh1qxijbodbas9z ~]$ echo '11 22 33' | xargs -E 33  echo 
    11 22
    [rookie@izbp18hovh1qxijbodbas9z ~]$


参考博客地址：
[xargs命令详解，xargs与管道的区别](https://www.cnblogs.com/wangqiguo/p/6464234.html)