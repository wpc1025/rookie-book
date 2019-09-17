# 事务

- Innodb引擎才支持事务

## 设置事务隔离级别

- 基于MySql 8.0版本
- 默认隔离级别为 可重复读`REPEATABLE-READ`

查询全局隔离级别设置
```
mysql> select @@GLOBAL.transaction_isolation;
+--------------------------------+
| @@GLOBAL.transaction_isolation |
+--------------------------------+
| READ-UNCOMMITTED               |
+--------------------------------+
1 row in set (0.00 sec)

mysql>
```

查询会话隔离级别设置
```
mysql> select @@SESSION.transaction_isolation;
+---------------------------------+
| @@SESSION.transaction_isolation |
+---------------------------------+
| REPEATABLE-READ                 |
+---------------------------------+
1 row in set (0.00 sec)
```

设置全局隔离级别
```
mysql> set GLOBAL TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

设置会话隔离级别
```
mysql> set SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```

| 隔离级别 | 描述 |
| :--- | :--- |
| READ UNCOMMITTED | 读未提交 |
| READ COMMITTED | 读提交 |
| REPEATABLE READ | 可重复读 |
| SERIALIZABLE | 串行 |


