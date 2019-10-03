# MySQL中的锁

> MySQL技术内幕：InnoDB存储引擎  
> [InnoDB Locking](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html)  
> [MySQL InnoDB锁机制全面解析分享](https://segmentfault.com/a/1190000014133576#articleHeader1)  
> [【MySQL】漫谈死锁](https://yq.aliyun.com/articles/282224)  
> [MySQL如何处理死锁](https://www.cnblogs.com/hhthtt/p/10707541.html)

## 一、锁

我们都知道`MySQL`的存储引擎是插件式的，支持`MyISAM`、`InnoDB`、`NDB`、`Memory`等存储引擎。不同的存储引擎支持不同的锁级别，比如，`MyISAM`支持表锁，
`Memory`支持表锁，`InnoDB`支持行锁。以下默认介绍的都是`InnoDB`存储引擎中的锁。

### 1.1 锁及其意义

锁机制用于管理对共享资源的并发访问。

当多个用户并发地存取数据时，在数据库中就可能会产生多个事务同时操作同一行数据的情况，若对并发操作不加控制就可能会读取和存储不正确的数据，破坏数据的一致性。

### 1.2 锁的种类
`InnoDB`存储引擎实现了两种标准的行级锁：共享锁、排他锁。同时，`InnoDB`为了实现多粒度锁定、允许事务在行级锁和表级锁共存，支持额外的表锁方式：意向共享锁、意向排他锁。


#### 1.2.1 排他锁（Exclusive Locks）

又称为写锁，指的是一个事务获取了一个数据行的排他锁，其他事务就不能再获取该行的其他锁，包括共享锁和排他锁，获得排他锁的事务可以对数据就行读取和修改。

我们来举个例子：

```sql
mysql> set autocommit=0;
Query OK, 0 rows affected (0.00 sec)

# 事务一
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user where id = 1 for update;
+----+----------+-----------+
| id | password | username  |
+----+----------+-----------+
|  1 | rookie   | 120522119 |
+----+----------+-----------+
1 row in set (0.00 sec)

# 事务二
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user where id = 1 for update;
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction

mysql> select * from user where id = 1 lock in share mode;
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
```

我们可以看到，当事务一为`id=1`的记录加上排他锁，且未提交事务时，事务二是无法再为`id=1`的记录加排他锁和共享锁的。

#### 1.2.2 共享锁（Shared Locks）

又称为读锁，是指多个事务对同一数据可以共享一把锁，都能访问到数据，但是只能读不能修改。

我们举个例子：
```sql
# 事务一
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user where id = 1 lock in share mode;
+----+----------+-----------+
| id | password | username  |
+----+----------+-----------+
|  1 | rookie   | 120522119 |
+----+----------+-----------+
1 row in set (0.00 sec)

# 事务二
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user where id=1 lock in share mode;
+----+----------+-----------+
| id | password | username  |
+----+----------+-----------+
|  1 | rookie   | 120522119 |
+----+----------+-----------+
1 row in set (0.00 sec)

mysql> select * from user where id = 1;
+----+----------+-----------+
| id | password | username  |
+----+----------+-----------+
|  1 | rookie   | 120522119 |
+----+----------+-----------+
1 row in set (0.00 sec)

mysql> select * from user where id=1 for update;
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction

mysql> update user set username='wpc' where id = 1;
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
```

我们从上面的例子可以看出，当为`id=1`的记录加上共享锁，且事务未提交时，其他事务是可以加共享锁及不加锁读取`id=1`的记录，但不可以更新记录以及加排他锁。

#### 1.2.3 意向排他锁（Intention Exclusive Locks）

事务想要获得一张表中某几行的排他锁。当为表的某行加上排他锁或共享锁之后，默认会为表加上意向排他锁或意向共享锁。意向锁不会与行级的共享/排他锁互斥，只会与表级的的共享锁、排他锁冲突。

我们举个例子：
```sql
# 事务一
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user where id=1 for update;
+----+----------+-----------+
| id | password | username  |
+----+----------+-----------+
|  1 | rookie   | 120522119 |
+----+----------+-----------+
1 row in set (0.00 sec)

# 事务二
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> lock tables user read;
^C -- query aborted
ERROR 1317 (70100): Query execution was interrupted
mysql> lock tables user write;
^C -- query aborted
ERROR 1317 (70100): Query execution was interrupted
mysql> select * from user where id=2 for update;
+----+-----------+----------+
| id | password  | username |
+----+-----------+----------+
|  2 | 120522119 | rookie   |
+----+-----------+----------+
1 row in set (0.00 sec)

mysql>

```

从以上例子我们可以知道：当为行加排他锁后，意向排他锁默认已经为表加上，这个时候是无法为表加排他锁、共享锁的，但是并不影响为其他行增加共享锁或排他锁。

#### 1.2.4 意向共享锁（Intention Shared Locks）

事务想要获得一张表中某几行的共享锁

我们来举个例子：

```sql
# 事务一
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user where id=1 lock in share mode;
+----+----------+-----------+
| id | password | username  |
+----+----------+-----------+
|  1 | rookie   | 120522119 |
+----+----------+-----------+
1 row in set (0.00 sec)

mysql>

# 事务二
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> lock tables user read;
Query OK, 0 rows affected (0.00 sec)

mysql> lock tables user write;
^C -- query aborted
ERROR 1317 (70100): Query execution was interrupted
mysql>
```
从以上例子我们可以知道：当为行加共享锁后，意向共享锁默认已经为表加上，这个时候是无法为表加排他锁的，但是为表加共享锁。

### 1.3 锁的三种算法

`InnoDB`存储引擎有3种行锁的算法，分别是：
- 记录锁（Record Locks）：单个行记录上的锁
- 间隙锁（Gap Locks）：锁定一个范围，但不包含记录本身
- 临键锁（Next-Key Locks）：记录锁+间隙锁，锁定一个范围，并且锁定记录本身

`InnoDB`所有的行锁算法都是基于索引实现的，锁定的也都是索引或索引区间

| 等值查询使用的索引类型 | 锁定内容 |
|:--- | :--- |
| 主键（聚簇索引） | 对聚簇索引记录+记录锁 |
| 唯一索引 | 对辅助索引记录+记录锁<br>对聚簇索引记录+记录锁 |
| 普通索引 | 对相关辅助索引+临键锁<br>对聚簇索引记录+记录锁 |
| 不使用索引 | 对聚簇索引全表+临键锁 |

### 1.4 锁问题

|锁问题|	锁问题描述|会出现锁问题的隔离级别|解决办法|
|:---|:---|:---|:---|
| 脏读 | 一个事务中会读到其他并发事务未提交的数据，违反了事务的隔离性 | Read Uncommitted | 提高事务隔离级别至Read Committed及以上 |
| 不可重复读 | 一个事务会读到其他并发事务已提交的数据，违反了数据库的一致性要求；<br>可能出现的问题为幻读，幻读是指在同一事务下，连续执行两次同样的SQL语句可能导致不同的结果，第二次的SQL语句可能返回之前不存在的行记录 | Read Uncommitted、Read Committed | 默认的RR隔离级别下 ，解决办法分为两种情况：1、当前读：Next-Key Lock机制对相关索引记录及索引间隙加锁，防止并发事务修改数据或插入新数据到间隙；<br>版本读：MVCC，保证事务执行过程中只有第一次读之前提交的修改和自己的修改可见，其他的均不可见；提高事务隔离级别至Serializable|
|丢失更新 | | Read Uncommitted、Read Committed、Repeatable Read | 默认的RR隔离级别下 ，解决办法分为两种情况：1、乐观锁：数据表增加version字段，读取数据时记录原始version，更新数据时，比对version是否为原始version，如不等，则证明有并发事务已更新过此行数据，则可回滚事务后重试直至无并发竞争；<br>2、悲观锁：读加排他锁，保证整个事务执行过程中，其他并发事务无法读取相关记录，直至当前事务提交或回滚释放锁|

### 1.5 死锁

#### 1.5.1 死锁原因
- 两个或者两个以上事务。
- 每个事务都已经持有锁并且申请新的锁。
- 锁资源同时只能被同一个事务持有或者不兼容。
- 事务之间因为持有锁和申请锁导致了循环等待。

#### 1.5.2 死锁的处理方式

MySQL有两种死锁处理方式：

1. 等待，直到超时（innodb_lock_wait_timeo   ut=50s）。
2. 发起死锁检测，主动回滚一条事务，让其他事务继续执行（innodb_deadlock_detect=on）。
由于性能原因，一般都是使用死锁检测来进行处理死锁。

**死锁检测**

死锁检测的原理是构建一个以事务为顶点、锁为边的有向图，判断有向图是否存在环，存在即有死锁。

**回滚**

检测到死锁之后，选择插入更新或者删除的行数最少的事务回滚，基于 INFORMATION_SCHEMA.INNODB_TRX 表中的 trx_weight 字段来判断。
#### 1.5.3 如何避免死锁

网上收集了一些注意事项，如下：
1. 使用事务，不使用 lock tables 。
2. 保证没有长事务。
3. 操作完之后立即提交事务，特别是在交互式命令行中。
4. 如果在用 (SELECT ... FOR UPDATE or SELECT ... LOCK IN SHARE MODE)，尝试降低隔离级别。
5. 修改多个表或者多个行的时候，将修改的顺序保持一致。
6. 创建索引，可以使创建的锁更少。
7. 最好不要用 (SELECT ... FOR UPDATE or SELECT ... LOCK IN SHARE MODE)。


