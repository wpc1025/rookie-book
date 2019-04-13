# 有序集合

与散列一样，都用于存储键值对：有序集合的键被称为成员，每个成员都是各不相同的；而有序集合的值则被称为分值，分值必须为浮点数。

| 命令 | 行为 |
| :--- | :--- |
| ZADD | 将一个带有指定分值的成员添加到有序集合里面 |
| ZRANGE | 根据元素在有序排列中所处的位置，从有序集合里获取多个元素 |
| ZRANGEBYSCORE | 获取有序集合在给定分值范围内的所有元素 |
| ZREM | 如果给定成员存在于有序集合，那么移除这个成员 |

**命令示例**

    127.0.0.1:6379> zadd zset-key 728 member1
    (integer) 1
    127.0.0.1:6379> zadd zset-key 982 member0
    (integer) 1
    127.0.0.1:6379> zadd zset-key 982 member0
    (integer) 0
    127.0.0.1:6379> zrange zset-key 0 -1 withscores
    1) "member1"
    2) "728"
    3) "member0"
    4) "982"
    127.0.0.1:6379> zrangebyscore zset-key 0 800 withscores
    1) "member1"
    2) "728"
    127.0.0.1:6379> zrem zset-key member1
    (integer) 1
    127.0.0.1:6379> zrem zset-key member1
    (integer) 0
    127.0.0.1:6379> zrange zset-key 0 -1 withscores
    1) "member0"
    2) "982"
    127.0.0.1:6379>
    


| 命令 | 行为 |
| :--- | :--- |
| ZCARD | 返回有序集合包含的成员数量 |
| ZINCRBY | 将member成员的分值加上increment |
| ZCOUNT | 返回分值介于min和max之间的成员数量 |
| ZRANK | 返回成员member在有序集合中的排名 |
| ZSCORE | 返回成员member的分值 |

**命令示例**

    127.0.0.1:6379> zadd zset-key 3 a 2 b 1 c
    (integer) 3
    127.0.0.1:6379> zcard zset-key
    (integer) 3
    127.0.0.1:6379> zincrby zset-key 3 c
    "4"
    127.0.0.1:6379> zscore zset-key c
    "4"
    127.0.0.1:6379> zrank zset-key c
    (integer) 2
    127.0.0.1:6379> zcount zset-key 0 3
    (integer) 2
    127.0.0.1:6379> zrange zset-key 0 -1 withscores
    1) "b"
    2) "2"
    3) "a"
    4) "3"
    5) "c"
    6) "4"
    127.0.0.1:6379> zrem zset-key b
    (integer) 1
    127.0.0.1:6379> zrange zset-key 0 -1 withscores
    1) "a"
    2) "3"
    3) "c"
    4) "4"
    127.0.0.1:6379>
    
| 命令 | 行为 |
| :--- | :--- |
| ZREVRANK | 返回有序集合里成员member的排名，成员按照分值从大到小排列 |
| ZREVRANGE | 返回有序集合给定排名范围内的成员，成员按照分值从大到小排列 |
| ZRANGEBYSCORE | 获取有序集合中分值介于min和max之间的所有成员 |
| ZREVRANGEBYSCORE | 获取有序集合中分值介于min和max之间的所有成员，并按照分值从大到小的顺序来返回它们 |
| ZREMRANGEBYRANK | 移除有序集合中排名介于min和max之间的所有成员 |
| ZREMRANGEBYSCORE | 移除有序集合中分值介于min和max之间的所有成员 |
| ZINTERSTORE | 对给定的有序集合执行类似于集合的交集运算 |
| ZUNIONSTORE | 对给定的有序集合执行类似于集合的并集运算 |

**命令示例**

    127.0.0.1:6379> zadd zset-1 1 a 2 b 3 c
    (integer) 3
    127.0.0.1:6379> zadd zset-2 4 b 1 c 0 d
    (integer) 3
    127.0.0.1:6379> zinterstore zset-i 2 zset-1 zset-2
    (integer) 2
    127.0.0.1:6379> zrange zset-i 0 -1 withscores
    1) "c"
    2) "4"
    3) "b"
    4) "6"
    127.0.0.1:6379>
    
