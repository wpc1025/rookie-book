# wget命令(2019.06.11)

- `wget`是一个下载文件的工具
- 体积小、功能完善、支持断点下载、支持FTP和HTTP下载方式、支持代理服务器

## 栗子

### 使用wget下载单个文件

    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ wget https://www.baidu.com/img/bd_logo1.png
    --2019-06-11 15:58:51--  https://www.baidu.com/img/bd_logo1.png
    Resolving www.baidu.com (www.baidu.com)... 115.239.211.112, 115.239.210.27
    Connecting to www.baidu.com (www.baidu.com)|115.239.211.112|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 7877 (7.7K) [image/png]
    Saving to: ‘bd_logo1.png’
    
    100%[===========================================================================================================>] 7,877       --.-K/s   in 0s      
    
    2019-06-11 15:58:51 (114 MB/s) - ‘bd_logo1.png’ saved [7877/7877]
    
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ ll
    total 8
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 bd_logo1.png
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ 

### 使用 wget -O下载并以不同的文件名保存

    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ wget -O baidu.png https://www.baidu.com/img/bd_logo1.png
    --2019-06-11 16:01:29--  https://www.baidu.com/img/bd_logo1.png
    Resolving www.baidu.com (www.baidu.com)... 115.239.210.27, 115.239.211.112
    Connecting to www.baidu.com (www.baidu.com)|115.239.210.27|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 7877 (7.7K) [image/png]
    Saving to: ‘baidu.png’
    
    100%[===========================================================================================================>] 7,877       --.-K/s   in 0s      
    
    2019-06-11 16:01:29 (80.6 MB/s) - ‘baidu.png’ saved [7877/7877]
    
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ ll
    total 16
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 baidu.png
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 bd_logo1.png
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ 

### 使用 wget --limit-rate 限速下载

当执行wget时，默认占用全部可能的宽带下载。

    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ wget --limit-rate=3K -O limit_rate_baidu.png https://www.baidu.com/img/bd_logo1.png
    --2019-06-11 16:19:33--  https://www.baidu.com/img/bd_logo1.png
    Resolving www.baidu.com (www.baidu.com)... 115.239.210.27, 115.239.211.112
    Connecting to www.baidu.com (www.baidu.com)|115.239.210.27|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 7877 (7.7K) [image/png]
    Saving to: ‘limit_rate_baidu.png’
    
    100%[===========================================================================================================>] 7,877       3.00KB/s   in 2.6s   
    
    2019-06-11 16:19:36 (3.00 KB/s) - ‘limit_rate_baidu.png’ saved [7877/7877]
    
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ ll
    total 24
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 baidu.png
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 bd_logo1.png
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 limit_rate_baidu.png
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ 
    
### 使用 wget -c 断点续传

    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ ll
    total 32
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 baidu.png
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 bd_logo1.png
    -rw-rw-r-- 1 rookie rookie 6662 Jun 11 16:22 http_baidu.png
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 limit_rate_baidu.png
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ wget -O http_baidu.png -c https://www.baidu.com/img/bd_logo1.png
    --2019-06-11 16:23:10--  https://www.baidu.com/img/bd_logo1.png
    Resolving www.baidu.com (www.baidu.com)... 115.239.210.27, 115.239.211.112
    Connecting to www.baidu.com (www.baidu.com)|115.239.210.27|:443... connected.
    HTTP request sent, awaiting response... 206 Partial Content
    Length: 7877 (7.7K), 1215 (1.2K) remaining [image/png]
    Saving to: ‘http_baidu.png’
    
    100%[+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++================>] 7,877       --.-K/s   in 0s      
    
    2019-06-11 16:23:10 (341 MB/s) - ‘http_baidu.png’ saved [7877/7877]
    
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ ll
    total 32
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 baidu.png
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 bd_logo1.png
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 http_baidu.png
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 limit_rate_baidu.png
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ 

### 使用 wget -b 后台下载

对于下载非常大的文件的时候，我们可以使用参数-b进行后台下载

    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ wget -O back_baidu.png -b --limit-rate=3k https://www.baidu.com/img/bd_logo1.png
    Continuing in background, pid 12908.
    Output will be written to ‘wget-log’.
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ cat wget-log 
    --2019-06-11 16:25:18--  https://www.baidu.com/img/bd_logo1.png
    Resolving www.baidu.com (www.baidu.com)... 115.239.210.27, 115.239.211.112
    Connecting to www.baidu.com (www.baidu.com)|115.239.210.27|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 7877 (7.7K) [image/png]
    Saving to: ‘back_baidu.png’
    
         0K .......                                               100% 3.00K=2.6s
    
    2019-06-11 16:25:20 (3.00 KB/s) - ‘back_baidu.png’ saved [7877/7877]
    
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ 

### 伪装代理名称下载

    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ wget -O agent_baidu.png --user-agent=”Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.204 Safari/534.16″  --limit-rate=3k https://www.baidu.com/img/bd_logo1.png
    -bash: syntax error near unexpected token `('
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ wget -O agent_baidu.png --user-agent='Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.204 Safari/534.16'  --limit-rate=3k https://www.baidu.com/img/bd_logo1.png
    --2019-06-11 16:36:45--  https://www.baidu.com/img/bd_logo1.png
    Resolving www.baidu.com (www.baidu.com)... 115.239.210.27, 115.239.211.112
    Connecting to www.baidu.com (www.baidu.com)|115.239.210.27|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 7877 (7.7K) [image/png]
    Saving to: ‘agent_baidu.png’
    
    100%[===========================================================================================================>] 7,877       3.00KB/s   in 2.6s   
    
    2019-06-11 16:36:48 (3.00 KB/s) - ‘agent_baidu.png’ saved [7877/7877]
    
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ ll
    total 52
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 agent_baidu.png
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 back_baidu.png
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 baidu.png
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 bd_logo1.png
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 http_baidu.png
    -rw-rw-r-- 1 rookie rookie 7877 Sep  3  2014 limit_rate_baidu.png
    -rw-rw-r-- 1 rookie rookie  482 Jun 11 16:25 wget-log
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ 

### 使用wget --spider测试下载链接

    [rookie@iZbp18hovh1qxijbodbas9Z wget]$ wget --spider https://www.baidu.com/img/bd_logo1.png
    Spider mode enabled. Check if remote file exists.
    --2019-06-11 16:39:06--  https://www.baidu.com/img/bd_logo1.png
    Resolving www.baidu.com (www.baidu.com)... 115.239.211.112, 115.239.210.27
    Connecting to www.baidu.com (www.baidu.com)|115.239.211.112|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 7877 (7.7K) [image/png]
    Remote file exists.
    
    [rookie@iZbp18hovh1qxijbodbas9Z wget]$
    
### 使用wget --tries增加重试次数

### <font color="red">使用 wget -i 下载多个文件</font>

保存一份下载链接文件，接着使用 `wget -i filelist.txt`

## 其他命令参数

| 参数 | 作用 |
| :---|:---|
| --mirror | 镜像网站 |
| --reject | 过滤指定格式下载 |
| -o | 把下载信息存入指定文件 |
| -Q | 限制总下载文件大小 |
| -r -A | 下载指定格式的文件 |
