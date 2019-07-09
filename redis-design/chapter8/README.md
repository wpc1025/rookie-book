# 对象

- Redis基于SDS、双端链表、字典、压缩列表、整数集合等数据结构创建了一个对象系统，该系统包含字符串对象、列表对象、哈希对象、集合对象和有序集合对象五种类型的对象，每种对象都用到了至少一种数据结构。
- Redis可以根据对象的类型来判断一个对象是否可以执行给定的命令
- 针对不同的使用场景，为对象设置多种不同的数据结构，优化对象在不同场景下的使用效率
- Redis对象系统基于引用计数回收内存、实现对象共享
- Redis对象记录访问时间记录信息，在服务器启用了`maxmemory`功能的情况下，空转时间较长的键可能会优先被服务器删除

## 一、对象的类型和编码

Redis的对象由`redisObject`结构表示

```C

typedef struct redisObject {

    // 类型
    unsigned type:4;
    
    // 编码
    unsigned encoding:4;
    
    // 指向底层实现数据结构的指针
    void *ptr;

} redisObject;

```

### 1.1 类型

对象的`type`属性记录了对象的类型，下表列出了`type`属性的值以及`TYPE`命令在面对不同类型的值对象时所产生的的输出

| 对象 | 对象type属性的值 | TYPE命令的输出 |
| :--- | :--- | :--- |
| 字符串对象 | REDIS_STRING | "string" |
| 列表对象 | REDIS_LIST | "list" |
| 哈希对象 | REDIS_HASH | "hash" |
| 集合对象 | REDIS_SET | "set" |
| 有序集合对象 | REDIS_ZSET | "zset" |

### 1.2 编码和底层实现

`encoding`属性记录了对象所使用的的编码，即这个对象使用什么数据结构作为对象的底层实现，下表列出了`encoding`属性可能的值：

| 编码常量 | 编码所对应的底层数据结构 |
| :--- | :--- |
| REDIS_ENCODING_INT | long类型的整数 |
| REDIS_ENCODING_EMBSTR | embstr编码的简单动态字符串 |
| REDIS_ENCODING_RAW | 简单动态字符串 |
| REDIS_ENCODING_HT | 字典 |
| REDIS_ENCODING_LINKEDLIST | 双端链表 |
| REDIS_ENCODING_ZIPLIST | 压缩列表 |
| REDIS_ENCODING_INTSET | 整数集合 |
| REDIS_ENCODING_SKIPLIST | 跳跃表和字典 |

每种类型的对象都至少使用了两种不同的编码，下表为每种类型的对象可以使用的编码

| 类型 | 编码 | 对象 |
| :--- | :--- | :--- |
| REDIS_STRING | REDIS_ENCODING_INT | 使用整数值实现的字符串对象 |
| REDIS_STRING | REDIS_ENCODING_EMBSTR | 使用embstr编码的简单动态字符串实现的字符串对象 |
| REDIS_STRING | REDIS_ENCODING_RAW | 使用简单动态字符串实现的字符串对象 |
| REDIS_LIST | REDIS_ENCODING_ZIPLIST | 使用压缩列表实现的列表对象 |
| REDIS_LIST | REDIS_ENCODING_LINKEDLIST | 使用双端列表实现的列表对象 |
| REDIS_HASH | REDIS_ENCODING_ZIPLIST | 使用压缩列表实现的哈希对象 |
| REDIS_HASH | REDIS_ENCODING_HT | 使用字典实现的哈希对象 |
| REDIS_SET | REDIS_ENCODING_INTSET | 使用整数集合实现的集合对象 |
| REDIS_SET | REDIS_ENCODING_HT | 使用字典实现的集合对象 |
| REDIS_ZSET | REDIS_ENCODING_ZIPLIST | 使用压缩列表实现的有序集合对象 |
| REDIS_ZSET | REDIS_ENCODING_SKIPLIST | 使用跳跃表和字典实现的有序集合对象 |





