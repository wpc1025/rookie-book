# 慢查询日志

Redis的慢查询日志功能用于记录执行时间超过给定时长的命令请求。使用先进先出的方式保存多条慢查询日志，当服务器存储的慢查询日志数量等于`slowlog-max-len`选项
的值时，服务器在添加一条新的慢查询日志之前，会将最旧的一条慢查询日志删除。

- `slowlog-log-slower-than`选项指定执行时间超过多少微秒的命令请求会被记录到日志上
- `slowlog-max-len`选项指定服务器最多保存多少条慢查询日志

可通过如下两条命令设置：
```
# 0微秒
CONFIG SET slowlog-log-slower-than 0
# 最多保存5条慢查询日志
CONFIG SET slowlog-max-len 5
# 查看服务器所保存的慢查询日志
SLOWLOG GET
```

## 一、慢查询记录的保存

```C
struct redisServer {
    // ...
    
    // 下一条慢查询日志的ID
    long slowlog_entry_id;
    
    // 保存了所有慢查询日志的链表
    list *slowlog;
    
    // 服务器配置 slowlog-log-slower-than 选项的值
    long slowlog-log-slower-than;
    
    // 服务器配置 slowlog-max-len 选项的值
    unsigned long slowlog_max_len;
    
    // ...
};

typedef struct slowlogEntry {
    // 唯一标识符
    long id;
    
    // 命令执行时的时间，格式为 UNIX 时间戳
    time_t time;
    
    // 执行命令消耗的时间，以微秒为单位
    long duration;
    
    // 命令与命令参数
    robj **argv;
    
    // 命令与命令参数的数量
    int argc;
    
} slowlogEntry;
```



