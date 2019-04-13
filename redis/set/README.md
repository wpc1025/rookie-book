# 集合

可以存储多个字符串，通过使用散列表来保证自己存储的每个字符串都是各不相同的，无序方式存储数据

| 命令 | 行为 |
| :--- | :--- |
| SADD | 将一个或多个元素添加到集合里面，并返回被添加元素当中原本并不存在于集合里面的元素数量 |
| SMEMBERS | 返回集合包含的所有元素 |
| SISMEMBER | 检查给定元素是否存在于集合之中 |
| SREM | 从集合里面移除一个或多个元素，并返回移除元素的数量 |

**命令示例**

    127.0.0.1:6379> sadd set-key item
    (integer) 1
    127.0.0.1:6379> sadd set-key item2
    (integer) 1
    127.0.0.1:6379> sadd set-key item3
    (integer) 1
    127.0.0.1:6379> sadd set-key item
    (integer) 0
    127.0.0.1:6379> smembers set-key
    1) "item3"
    2) "item"
    3) "item2"
    127.0.0.1:6379> sismember set-key item4
    (integer) 0
    127.0.0.1:6379> sismember set-key item
    (integer) 1
    127.0.0.1:6379> srem set-key item2
    (integer) 1
    127.0.0.1:6379> srem set-key item2
    (integer) 0
    127.0.0.1:6379> smembers set-key
    1) "item3"
    2) "item"
    127.0.0.1:6379>
    
    
   
| 命令 | 行为 |
| :--- | :--- |
| SCARD | 返回集合包含的元素的数量 |
| SRANDMEMBER | 从集合里面随机的返回一个或多个元素，当count为正数，命令返回的随机元素不会重复，当count为负数，命令返回的随机元素可能会出现重复 |
| SPOP | 随机的移除集合中的一个元素，并返回被移除的元素 |
| SMOVE | 如何集合source-key包含元素item，那么从集合source-key里面移除元素item，并将元素item添加到集合dest-key中，如果item被成功移除，那么命令返回1，否则返回0 |

**命令示例**

    127.0.0.1:6379> sadd set-key a b c
    (integer) 3
    127.0.0.1:6379> srem set-key c d
    (integer) 1
    127.0.0.1:6379> scard set-key
    (integer) 2
    127.0.0.1:6379> smembers set-key
    1) "b"
    2) "a"
    127.0.0.1:6379> smove set-key set-key2 a
    (integer) 1
    127.0.0.1:6379> smove set-key set-key2 c
    (integer) 0
    127.0.0.1:6379> smembers set-key
    1) "b"
    127.0.0.1:6379>
    
| 命令 | 行为 |
| :--- | :--- |
| SDIFF | 返回那些存在于第一个集合，但不存在于其他集合中的元素（差集运算） |
| SDIFFSTORE | 将那些存在于第一个集合，但并不存在于其他集合中的元素存储到dest-key里面 |
| SINTER | 返回那些同时存在于所有集合中的元素（交集运算） |
| SINTERSTORE | 将那些同时存在于所有集合中的元素存储到dest-key键里面 |
| SUNION | 返回那些至少存在于一个集合中的元素（并集运算） |
| SUNIONSTORE | 将那些至少存在于一个集合中的元素存储到dest-key键里面 |


**命令示例**

    127.0.0.1:6379> sadd skey1 a b c d
    (integer) 4
    127.0.0.1:6379> sadd skey2 c d e f
    (integer) 4
    127.0.0.1:6379> sdiff skey1 skey2
    1) "a"
    2) "b"
    127.0.0.1:6379> sinter skey1 skey2
    1) "d"
    2) "c"
    127.0.0.1:6379> sunion skey1 skey2
    1) "f"
    2) "d"
    3) "c"
    4) "b"
    5) "e"
    6) "a"
    127.0.0.1:6379>
    
