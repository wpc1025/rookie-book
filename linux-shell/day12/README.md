# tar命令(2019.06.15)

- `tar`命令可以为Linux的文件和目录创建档案
- 利用`tar`命令，可以把一大堆文件和目录打包成一个文件

**打包和压缩的区别：**
- 打包是将一大堆文件和目录变成一个总的文件
- 压缩是将一个大的文件通过一些压缩算法变成一个小文件

## 一、语法

`tar (选项) (参数)`

## 二、选项

| 选项 | 作用 |
| :--- | :--- |
| -A 或 --catenate | 新增文件到已存在的备份文件 |
| -B | 设置区块大小 |
| -c 或 --create | 建立新的备份文件 |
| -C <目录> | 用在解压缩，若在特定目录解压缩，可以使用这个选项 |
| -d | 记录文件的差别 |
| -x | 从备份文件中还原文件 |
| -t | 列出备份文件的内容 |
| -z | 通过gzip指令处理备份文件 |
| -Z | 通过compress指令处理备份文件 |
| -f <备份文件> | 指定备份文件 |
| -v | 显示指令执行过程 |
| -r | 添加文件到已经压缩的文件 | 
| -u | 添加改变了现有的文件到已经存在的压缩文件 | 
| -j | 支持bzip2解压文件 |
| -l | 文件系统边界设置 |
| -k | 保留原有文件不覆盖 |
| -m | 保留文件不被覆盖 |
| -w | 确认压缩文件的确认性 |
| -p | 用原来的文件权限还原文件 |
| -P | 文件名使用绝对名称，不移除文件名称前的"/"号 |
| -N <日期格式> | 只将较指定日期更新的文件保存到备份文件里 |
| --exclude=<范式样式> | 排除符合范式样式的文件 |

## 三、参数

文件或目录：指定要打包的文件或目录列表

## 四、栗子

### 4.1 将文件全部打成tar包

#### 4.1.1 仅打包不压缩

     tar -cvf rookie.tar rookie.log

#### 4.1.2 打包后，以gzip压缩

    tar -zcvf rookie_log.tar.gz rookie.log 
    
#### 4.1.3 打包后，以bzip2压缩

    tar -jcvf rookie_log.tar.bz2 rookie.log
    
### 4.2 查阅tar包内的文件

    [root@iZbp18hovh1qxijbodbas9Z logs]# tar -tzvf rookie_log.tar.gz
    -rw-r--r-- root/root      3756 2019-03-28 09:25 rookie.log
    [root@iZbp18hovh1qxijbodbas9Z logs]# 

### 4.3 解压缩tar包

    tar -zxvf ./logs/rookie_log.tar.gz
    
### 4.4 将tar内的部分文件解压出来

    tar -zxvf ./logs/rookie_log.tar.gz rookie.log
    

  