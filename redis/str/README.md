# 字符串

字符串可以存储以下3种值
- 字符串
- 整数
- 浮点数

| 命令 | 行为 |
| :--- | :---|
| GET | 获取存储在给定键中的值 |
| SET | 设置存储在给定键中的值 |
| DEL | 删除存储在给定键中的值（这个命令可以用于所有类型） |
| INCR | 将键存储的值加1 |
| DECR | 将键存储的值减1 |
| INCRBY | 将键存储的值加上整数amount |
| DECRBY | 将键存储的值减去整数amount |
| INCRBYFLOAT | 将键存储的值加上浮点数amount |



**命令实例**

    127.0.0.1:6379> set str-key 100
    OK
    127.0.0.1:6379> get str-key
    "100"
    127.0.0.1:6379> incr str-key
    (integer) 101
    127.0.0.1:6379> incrby str-key 9
    (integer) 110
    127.0.0.1:6379> decr str-key
    (integer) 109
    127.0.0.1:6379> decrby str-key 9
    (integer) 100
    127.0.0.1:6379> incrbyfloat str-key 10.25
    "110.25"
    127.0.0.1:6379> get str-key
    "110.25"
    127.0.0.1:6379> del str-key
    (integer) 1
    127.0.0.1:6379> get str-key
    (nil)
    127.0.0.1:6379>

| 命令 | 行为 |
| :--- | :---|
| APPEND | 将值追加到给定键当前存储的值的末尾 |
| GETRANGE | 获取一个由偏移量start至偏移量end范围内所有字符组成的子串，包括start和end在内 |
| SETRANGE | 将从start偏移量开始的子串设置为给定值 |
| GETBIT | 将字符串看作是二进制位串，并返回串中偏移量为offset的二进制位的值 |
| SETBIT | 将字符串看作是二进制位串，并将位串中偏移量为offset的二进制位的值设置为value |
| BITCOUNT | 统计二进制位串里面值为1的二进制位的数量，如果给定了可选的start偏移量和end偏移量，那么只对偏移量指定范围内的二进制位进行统计 |
| BITOP | 对一个或多个二进制位串执行包括并`AND`、或`OR`、异或`XOR`、非`NOT`在内的任意一种按位运算操作，并将计算结果保存在dest-key键里面 |

**命令示例**

    127.0.0.1:6379> append new-string-key "hello "
    (integer) 6
    127.0.0.1:6379> append new-string-key world!
    (integer) 12
    127.0.0.1:6379> substr new-string-key 3 7
    "lo wo"
    127.0.0.1:6379> setrange new-string-key 0 H
    (integer) 12
    127.0.0.1:6379> setrange new-string-key 6 W
    (integer) 12
    127.0.0.1:6379> get new-string-key
    "Hello World!"
    127.0.0.1:6379> setrange new-string-key 11 ", how are you?"
    (integer) 25
    127.0.0.1:6379> get new-string-key
    "Hello World, how are you?"
    127.0.0.1:6379> setbit new-string-key 2 1
    (integer) 0
    127.0.0.1:6379> setbit new-string-key 7 1
    (integer) 0
    127.0.0.1:6379> get new-string-key
    "iello World, how are you?"
    127.0.0.1:6379>