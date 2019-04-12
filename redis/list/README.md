# 列表

一个列表结构可以有序的存储多个字符串

| 命令 | 行为 |
| :--- | :--- |
| RPUSH | 将给定值推入列表的右端 |
| LRANGE | 获取列表在给定范围上的所有值 |
| LINDEX | 获取列表在给定位置上的单个元素 |
| LPOP | 从列表的左端弹出一个值，并返回被弹出的值 |

**命令示例**

    127.0.0.1:6379> rpush list-key item
    (integer) 1
    127.0.0.1:6379> rpush list-key item2
    (integer) 2
    127.0.0.1:6379> rpush list-key item
    (integer) 3
    127.0.0.1:6379> lrange list-key 0 -1
    1) "item"
    2) "item2"
    3) "item"
    127.0.0.1:6379> lindex list-key 1
    "item2"
    127.0.0.1:6379> lpop list-key
    "item"
    127.0.0.1:6379> lrange list-key 0 -1
    1) "item2"
    2) "item"
    127.0.0.1:6379>

| 命令 | 行为 |
| :--- | :--- |
| RPUSH | 将一个或多个值推入列表的右端 |
| LPUSH | 将一个或多个值推入列表的左端 |
| RPOP | 移除并返回列表最右端的元素 |
| LPOP | 移除并返回列表最左端的元素 |
| LINDEX | 返回列表中偏移量为offset的元素 |
| LRANGE | 返回列表从start偏移量到end偏移量范围内的所有元素，其中偏移量start和偏移量end的元素也会包含在被返回的元素之中 |
| LTRIM | 对列表进行修剪，只保留从start偏移量到end偏移量范围内的元素，其中偏移量为start和偏移量end的元素也会保留 |

**命令示例**

    127.0.0.1:6379> rpush list-key last
    (integer) 1
    127.0.0.1:6379> lpush list-key first
    (integer) 2
    127.0.0.1:6379> lrange list-key 0 -1
    1) "first"
    2) "last"
    127.0.0.1:6379> lpop list-key
    "first"
    127.0.0.1:6379> lrange list-key 0 -1
    1) "last"
    127.0.0.1:6379> rpush list-key a b c
    (integer) 4
    127.0.0.1:6379> lrange list-key 0 -1
    1) "last"
    2) "a"
    3) "b"
    4) "c"
    127.0.0.1:6379> ltrim list-key 2 -1
    OK
    127.0.0.1:6379> lrange list-key 0 -1
    1) "b"
    2) "c"
    127.0.0.1:6379>

阻塞式的列表弹出命令以及在列表之间移动元素的命令

| 命令 | 行为 |
| :--- | :--- |
| BLPOP | 从第一个非空列表中弹出位于最左端的元素，或者在timeout秒之内阻塞并等待可弹出的元素出现 |
| BRPOP | 从第一个非空列表中弹出位于最右端的元素，或者在timeout秒之内阻塞并等待可弹出的元素出现 |
| RPOPLPUSH | 从source-key列表中弹出位于最右端的元素，然后将这个元素推入dest-key列表的最左端，并向用户返回这个元素 |
| BRPOPLPUSH | 从source-key列表中弹出位于最右端的元素，然后将这个元素推入dest-key列表的最左端，并向用户返回这个元素，如果source-key为空，那么在timeout秒之内阻塞并等待可弹出的元素出现 |

**命令示例**

    127.0.0.1:6379> rpush list item1
    (integer) 1
    127.0.0.1:6379> rpush list item2
    (integer) 2
    127.0.0.1:6379> rpush list2 item3
    (integer) 1
    127.0.0.1:6379> brpoplpush list2 list 1
    "item3"
    127.0.0.1:6379> brpoplpush list2 list 1
    (nil)
    (1.09s)
    127.0.0.1:6379> blpop list list2 1
    1) "list"
    2) "item3"
    127.0.0.1:6379> blpop list list2 1
    1) "list"
    2) "item1"
    127.0.0.1:6379> blpop list list2 1
    1) "list"
    2) "item2"
    127.0.0.1:6379> blpop list list2 1
    (nil)
    (1.05s)
    127.0.0.1:6379>
