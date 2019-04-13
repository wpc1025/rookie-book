# 散列

可以存储多个键值对之间的映射

| 命令 | 行为 |
| :--- | :--- |
| HSET | 在散列里面关联起给定的键值对 |
| HGET | 获取指定散列键的值 |
| HGETALL | 获取散列包含的所有键值对 |
| HDEL | 如果给定键存在于散列里面，那么移除这个键 |


    127.0.0.1:6379> hset hash-key sub-key1 value1
    (integer) 1
    127.0.0.1:6379> hset hash-key sub-key2 value2
    (integer) 1
    127.0.0.1:6379> hset hash-key sub-key1 value1
    (integer) 0
    127.0.0.1:6379> hgetall hash-key
    1) "sub-key1"
    2) "value1"
    3) "sub-key2"
    4) "value2"
    127.0.0.1:6379> hdel hash-key sub-key2
    (integer) 1
    127.0.0.1:6379> hdel hash-key sub-key2
    (integer) 0
    127.0.0.1:6379> hgetall hash-key
    1) "sub-key1"
    2) "value1"
    127.0.0.1:6379>
    
| 命令 | 行为 |
| :--- | :--- |
| HMGET | 从散里面获取一个或多个键的值 |
| HMSET | 为散列里面的一个或多个键设置值 |
| HLEN | 返回散列里面包含的键值对的数量 |


**命令示例**

    127.0.0.1:6379> hmset hash-key k1 v1 k2 v2 k3 v3
    OK
    127.0.0.1:6379> hmget hash-key k2 k3
    1) "v2"
    2) "v3"
    127.0.0.1:6379> hlen hash-key
    (integer) 3
    127.0.0.1:6379> hgetall hash-key
    1) "k1"
    2) "v1"
    3) "k2"
    4) "v2"
    5) "k3"
    6) "v3"
    127.0.0.1:6379> hdel hash-key k1 k3
    (integer) 2
    127.0.0.1:6379> hgetall hash-key
    1) "k2"
    2) "v2"
    127.0.0.1:6379>
    
| 命令 | 行为 |
| :--- | :--- |
| HEXISTS | 检查给定键是否存在于散列中 |
| HKEYS | 获取散列包含的所有键 |
| HVALS | 获取散列包含的所有值 |
| HINCRBY | 将键key存储的值加上整数increment |
| HINCRBYFLOAT | 将键key存储的值加上浮点数increment |


**命令示例**

    127.0.0.1:6379> hmset hash-key2 short hello long 1000*1
    OK
    127.0.0.1:6379> hkeys hash-key2
    1) "short"
    2) "long"
    127.0.0.1:6379> hexists hash-key2 num
    (integer) 0
    127.0.0.1:6379> hincrby hash-key2 num 1
    (integer) 1
    127.0.0.1:6379> hexists hash-key2 num
    (integer) 1
    127.0.0.1:6379> hgetall hash-key2
    1) "short"
    2) "hello"
    3) "long"
    4) "1000*1"
    5) "num"
    6) "1"
    127.0.0.1:6379>
    
