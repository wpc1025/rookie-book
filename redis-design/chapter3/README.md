# 字典

- 又称为符号表、关联数组、映射
- 一种保存键值对的数据结构
- 字典中每个健都是独一无二的
- Redis中的数据库、哈希键等用到了字典

## 一、字典的实现

字典使用哈希表作为底层实现，一个哈希表包含多个哈希节点，每个哈希节点保存了字典的一个键值对

### 1.1 哈希表

```C

typedef struct dictht {

    // 哈希表数组
    dictEntry **table;
    
    // 哈希表大小
    unsigned long size;
    
    // 哈希表大小掩码，用来计算索引值
    // 总是等于 size - 1
    unsigned long sizemask;
    
    // 该哈希表已有节点数量
    unsigned long used;

} dictht;

```

`sizemask`属性的值总是等于`size - 1`，这个属性和哈希值一起决定一个键应该被放到table数组的哪个索引上面

**空哈希表图示如下**

![空哈希表图示](../images/zd/empty-dic.png)

### 1.2 哈希表节点

```C

typedef struct dictEntry {

    // 键
    void *key;
    
    // 值
    union{
        void *val;
        uint64_tu64;
        int64_ts64;
    } v;
    
    // 指向下一个哈希表节点，形成链表
    struct dictEntry *next;

} dictEntry;

```

- `v`属性保存着键值对中的值，其中键值对的值可以是一个指针，或者是一个`uint64_t`整数，又或者是一个`int64_t`整数
- `next`属性是指向另一个哈希表节点的指针，这个指针可以将多个哈希值相同的键值对连接在一起，以此来解决键冲突问题

### 1.3 字典

```C

typedef struct dict {

    // 类型特定函数
    dictType *type;
    
    // 私有数据
    void *privdata;
    
    // 哈希表
    dictht ht[2];
    
    // rehash索引
    // 当rehash不在进行时，值为-1
    long rehashidx;

} dict;

```

`type`属性和`privdata`属性是针对不同类型的键值对，为创建多态字典而设置的：
- `type`属性是一个指向`dictType`结构的指针，每个`dictType`结构保存了一簇用于操作特定类型键值对的函数，Redis会为用途不同的字典设置不同的类型特定函数
- `privdata`属性保存了需要传给那些类型特定函数的可选参数
- `ht`属性是一个包含两个项的数组，数组的每一项都是一个`dictht`哈希表，一般情况下，字典只使用`ht[0]`哈希表，`ht[1]`哈希表只会在对`ht[0]`哈希表进行`rehash`时使用
- `rehash`属性记录`rehash`目前的进度，如果目前没有`rehash`，那么它的值为-1

**普通状态下字典图示：**

![普通状态下字典图示](../images/zd/normal-dict.png)

## 二、哈希算法

```C

    // 使用字典设置的哈希函数，计算键key的哈希值
    hash = dict -> type -> hashFunction(key);
    
    // 使用哈希表的sizemask属性和哈希值，计算出索引值
    // 根据情况不同，ht[x]可以是ht[0]或者ht[1]
    index = hash & dict -> ht[x].sizemask;

```

**注意**

当字典用作数据库的底层实现，或者哈希键的底层实现时，Redis使用`MurmurHash2`算法来计算键的哈希值。因为`MurmurHash2`算法对于有规律的键，仍能给出一个很好的随机分布性。

## 三、解决键冲突












