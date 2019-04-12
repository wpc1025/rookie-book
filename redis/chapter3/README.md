# 第3章 Redis命令

## 3.1 字符串

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

