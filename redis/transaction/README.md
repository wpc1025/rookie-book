# Redis事务

Redis的基本事务需要用到MULTI命令和EXEC命令，这种事务可以让一个客户端在不被其他客户端打断的情况下执行多个命令。

要在Redis里面执行任务，我们首先需要执行MULTI命令，然后输入那些我们想要在事务里面执行的命令，最后再执行EXEC命令。


    127.0.0.1:6379> multi
    OK
    127.0.0.1:6379> sadd user:1:following 2
    QUEUED
    127.0.0.1:6379> sadd user:2:following 1
    QUEUED
    127.0.0.1:6379> exec
    1) (integer) 1
    2) (integer) 1
    127.0.0.1:6379> smembers user:1:following
    1) "2"
    127.0.0.1:6379> smembers user:2:following
    1) "1"
    127.0.0.1:6379>
    
