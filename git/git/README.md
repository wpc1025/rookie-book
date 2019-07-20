# git

## 一、关于版本控制

版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定修订情况的系统。

1. 本地版本控制系统
2. 集中化的版本控制系统

    单一的集中管理的服务器，保存所有文件的修订版本，协同工作的人们都通过客户端连接到这台服务器，取出最新的文件或提交更新
    
3. 分布式版本控制系统

    客户端并不只是提取最新版本的文件快照，而是把代码仓库完整的镜像下来。这样一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。
    
## 二、git基础

git与其他版本控制系统的区别：

1. 直接记录快照，而非差异比较
    - 其他大部分系统以文件变更列表的方式存储信息，这类系统将他们保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异
    - git会对当时的全部文件制作一个快照并保存这个快照的索引。若其中某个文件没有被修改，则只保留该文件的链接指向之前存储的文件
2. 近乎所有操作都是本地执行
3. git保证完整性
4. git一般只添加数据

## 三、初次运行git前的配置

git的配置变量存储在三个地方：

1. `/etc/gitconfig`文件：包括系统上每一个用户及其仓库的通用配置，如果使用`--system`选项的`git config`时，会从该文件读写配置
2. `~/.gitconfig`或`~/.config/git/config`文件：只针对当前用户，可以传递`--global`选项让git读写此文件
3. 当前使用仓库的Git目录中的`config`文件：针对该仓库

### 3.1 配置常用命令
```
# 设置用户名邮箱
git config --global user.name "rookie"
git config --global user.email "11957@qq.com"

# 显示所有配置
git config --list

# 显示配置的user.name
git config user.name

```

## 四、获取git仓库

1. 在现有目录初始化仓库
    - 空文件夹
        ```
        git init
        ```
    - 非空文件夹
        ```
            git init 
            git add *
            git commit -m 'init commit'
        ```
2. 克隆现有的仓库

```
git clone https://github.com/libgit2/libgit2
```

### 4.1 git常用命令

```
# 检查当前文件状态
git status

# 状态简览
git status --short
git status -s

# 开始跟踪新文件、把已跟踪的文件放入暂存区、标记为冲突已解决状态
git add

# 比较工作目录中当前文件和暂存区域快照之间的差异
git diff

# 查看已暂存的将要添加到下次提交的内容
git diff --cached

# 提交暂存区中的文件
git commit 
git commit -v
git commit -m 'README'
# 跳过使用暂存区，自动把所有已经跟踪过的文件暂存起来一并提交
git commit -a

# 从已跟踪文件中移除
git rm
# 从已跟踪文件中强制移除
git rm -f
# 从已跟踪文件中移除，但保留本地
git rm --cached 
# 可以使用表达式
git rm log/\*.log

# mv命令
git mv file_from file_to

```

1. 状态简览

    使用`git status -s`或者`git status --short`命令可以简单显示当前文件的状态
    
    ```
    # 文件被修改但还没有放入暂存区
     M README
     
    # 文件被修改放到暂存区之后又被修改了
    MM Rakefile
    
    # 文件新添加到暂存区
    A  lib/git.rb
    
    # 文件被修改并放入了暂存区
    M  lib/simplegit.rb
    
    # 新添加的未跟踪文件
    ?? LICENSE.txt
    ```

2. 忽略文件

    `.gitignore`文件可以用来设置哪些文件无需纳入Git的管理
    
    ```
    * 匹配零个或多个匹配符
    [abc] 匹配任意一个列在括号中的字符
    ? 只匹配一个任意字符
    [0-9] 匹配所有0到9的数字
    a/**/z 使用两个*号标识匹配任意中间目录
    ```
    
    例子如下：
    
    ```
    # no .a files
    *.a
    
    # but do track lib.a, even though you're ignoring .a files above
    !lib.a
    
    # only ignore the TODO file in the current directory, not subdir/TODO
    /TODO
    
    # ignore all files in the build/ directory
    build/
    
    # ignore doc/notes.txt, but not doc/server/arch.txt
    doc/*.txt
    
    # ignore all .pdf files in the doc/ directory
    doc/**/*.pdf
    ```

## 五、查看提交历史

`git log`
