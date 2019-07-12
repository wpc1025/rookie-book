# source命令(2019.07.11)

- `source`命令可用于将任何函数文件加载到当前shell脚本或命令提示符中
- 从给定的`FILENAME`读取命令并返回
- `$path`中的路径名用于查找包含`FILENAME`的目录。如果提供了任何`ARGUMENTS`，它们将成为执行`FILENME`时的位置参数

## 一、语法

`source filename [arguments]`

## 二、栗子

1. 创建如下内容的`mylib.sh`脚本

    ```
    #!/bin/bash
    
    JATL_ROOT=/wwww/httpd
    is_root(){
        [ $(id -u) -eq 0 ] && return $TRUE || return $FALSE
    }
    ```

2. 创建`test.sh`脚本，在脚本中调用`is_root()`

    ```
    #!/bin/bash
    # Load the mylib.sh using source command
    source mylib.sh
    
    echo "JATL_ROOT is set to $JATL_ROOT"
    
    is_root && echo "You are logged in as root." || echo "You are not logged in as root."
    ```

3. 修改为可执行文件并执行

    ```
    [rookie@izbp18hovh1qxijbodbas9z ~]$ chmod +x test.sh 
    [rookie@izbp18hovh1qxijbodbas9z ~]$ ./test.sh 
    JATL_ROOT is set to /wwww/httpd
    You are not logged in as root.
    [rookie@izbp18hovh1qxijbodbas9z ~]$ 
    ```

## 三、关于返回状态的注意事项

`source`命令返回在`FILENAME`中执行的最后一个命令的状态；如果无法读取`FILENAME`，则会失败。

退出状态0表示源文件成功读取

退出状态1表示源文件无法读取
