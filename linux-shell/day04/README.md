# find命令(2019.06.07)

- 强大搜索命令，可按照文件名、权限、大小、时间、inode号搜索文件
- 直接在硬盘中进行搜索，占用过多资源，所以不要指定较大的搜索范围

## 命令格式

`find 搜索路径 [选项] 搜索内容`

## 命令参数

| 参数 | 作用 | 
| :--- | :--- |
| -name | 按照文件名搜索 |
| -iname | 按照文件名搜索，不区分大小写 |
| -inum | 按照inode号搜索 |
| -size [+-] | 按照指定文件大小搜索内容 |
| -atime [+-] | 按照文件访问时间搜索 |
| -mtime [+-] | 按照文件修改时间搜索 |
| -perm [+-] | 按照文件权限进行搜索 |
|-uid |用户 ID:按照用户 ID 査找所有者是指定 ID 的文件|
|-gid |组 ID:按照用户组 ID 査找所属组是指定 ID 的文件|
|-user| 用户名：按照用户名査找所有者是指定用户的文件|
|-group |组名：按照组名査找所属组是指定用户组的文件|
|-nouser|査找没有所有者的文件|
|-type d|查找目录|
|-type f|查找普通文件|
|-type l|查找软链接文件|
|-a|and逻辑与|
|-o|or逻辑或|
|-not|not逻辑非|


## 栗子

### -name

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ find . -name wechat-spring-boot
    ./wechat-spring-boot
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
### -iname

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ find . -name wechat-spring-boot
    ./wechat-spring-boot
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ find . -iname wechat-spring-boot
    ./wechat-spring-boot
    ./Wechat-Spring-Boot
    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ 
    
### -inode

    [rookie@iZbp18hovh1qxijbodbas9Z wechat-spring-boot]$ ls -i wechat-spring-boot.jar
    657525 wechat-spring-boot.jar
    [rookie@iZbp18hovh1qxijbodbas9Z wechat-spring-boot]$ find . -inum 657525
    ./wechat-spring-boot.jar
    [rookie@iZbp18hovh1qxijbodbas9Z wechat-spring-boot]$ 

### -size

    [rookie@iZbp18hovh1qxijbodbas9Z 1]$ ll -h 
    total 72K
    -rw-rw-r-- 1 rookie rookie  143 Jun 30  2018 canChangeFun.py
    -rw-rw-r-- 1 rookie rookie  120 Jun 30  2018 canChangeNameFun.py
    -rw-rw-r-- 1 rookie rookie  256 Jul  2  2018 classTest.py
    -rw-rw-r-- 1 rookie rookie  304 Jul  2  2018 decoratorTest.py
    -rwxrwxr-x 1 rookie rookie  123 Jun 29  2018 deftest.py
    -rw-rw-r-- 1 rookie rookie  298 Jul  9  2018 exception.py
    -rwxrwxr-x 1 rookie rookie  106 Jun 29  2018 forIn.py
    -rwxrwxr-x 1 rookie rookie  382 Jul 18  2018 forkTest.py
    -rw-rw-r-- 1 rookie rookie   47 Jul  1  2018 generatorTest.py
    -rw-rw-r-- 1 rookie rookie  337 Jul  2  2018 helloMoudle.py
    -rwxrwxr-x 1 rookie rookie   44 Jun 29  2018 hello.py
    -rwxrwxr-x 1 rookie rookie  175 Jun 29  2018 if.py
    -rw-rw-r-- 1 rookie rookie  119 Jun 30  2018 keyFun.py
    -rw-rw-r-- 1 rookie rookie  121 Jun 30  2018 nameKeyFun.py
    -rw-rw-r-- 1 rookie rookie  143 Jun 30  2018 power.py
    drwxrwxr-x 2 rookie rookie 4.0K Jul  2  2018 __pycache__
    -rwxrwxr-x 1 rookie rookie  132 Jul 19  2018 subprocessTest.py
    -rwxrwxr-x 1 rookie rookie  120 Jun 29  2018 while.py
    [rookie@iZbp18hovh1qxijbodbas9Z 1]$ find . -size 4k
    .
    ./__pycache__
    [rookie@iZbp18hovh1qxijbodbas9Z 1]$ find . -size -4k
    ./hello.py
    ./while.py
    ./generatorTest.py
    ./__pycache__/deftest.cpython-37.pyc
    ./__pycache__/canChangeFun.cpython-37.pyc
    ./__pycache__/decoratorTest.cpython-37.pyc
    ./__pycache__/classTest.cpython-37.pyc
    ./__pycache__/nameKeyFun.cpython-37.pyc
    ./__pycache__/canChangeNameFun.cpython-37.pyc
    ./__pycache__/power.cpython-37.pyc
    ./__pycache__/keyFun.cpython-37.pyc
    ./classTest.py
    ./if.py
    ./deftest.py
    ./forkTest.py
    ./decoratorTest.py
    ./helloMoudle.py
    ./exception.py
    ./keyFun.py
    ./subprocessTest.py
    ./forIn.py
    ./nameKeyFun.py
    ./canChangeFun.py
    ./power.py
    ./canChangeNameFun.py
    [rookie@iZbp18hovh1qxijbodbas9Z 1]$ find . -size +4k
    [rookie@iZbp18hovh1qxijbodbas9Z 1]$ 


