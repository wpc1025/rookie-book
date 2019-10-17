# MySQL中的锁

> MySQL技术内幕：InnoDB存储引擎  
> [MySQL 加锁处理分析](https://www.cnblogs.com/tutar/p/5878651.html)  
> [MySQL索引和SQL调优](https://juejin.im/post/5a6873fbf265da3e393a97fa#comment)  
> [mysql死锁问题分析](https://yq.aliyun.com/articles/5533?&utm_source=qq)  

## 一、锁

我们都知道`MySQL`的存储引擎是插件式的，支持`MyISAM`、`InnoDB`、`NDB`、`Memory`等存储引擎。不同的存储引擎支持不同的锁级别，比如，`MyISAM`支持表锁，
`Memory`支持表锁，`InnoDB`支持行锁。以下默认介绍的都是`InnoDB`存储引擎中的锁。

### 1.1 锁及其意义

锁机制用于管理对共享资源的并发访问。

当多个用户并发地存取数据时，在数据库中就可能会产生多个事务同时操作同一行数据的情况，若对并发操作不加控制就可能会读取和存储不正确的数据，破坏数据的一致性。

### 1.2 锁的种类
`InnoDB`存储引擎实现了两种标准的行级锁：共享锁、排他锁。同时，`InnoDB`为了实现多粒度锁定、允许事务在行级锁和表级锁共存，支持额外的表锁方式：意向共享锁、意向排他锁。

#### 1.2.1 如何查看锁
在`MySQL 8.0`中，`performance_schema.data_locks`表存储了持有和请求的数据锁。
```sql
SELECT * FROM performance_schema.data_locks;
```
- `LOCK_TYPE`：锁的类型，对应`InnoDB`，允许的值`RECORD`用于行级锁，`TABLE`用于表级锁。
- `LOCK_MODE`：锁定的模式，对应`InnoDB`时，允许值是 S[,GAP]，X[,GAP]， IS[,GAP]，IX[,GAP]， AUTO_INC，和 UNKNOWN。
- `LOCK_STATUS`：锁的状态，对应`InnoDB`，`GRANTED`表示持有锁，`WAITING`代表等待锁。

在`MySQL 8.0`中，`performance_schema.data_lock_waits`表显示了`data_locks`表中的哪些数据锁定请求被表中的哪些保留数据锁定阻止。
```sql
SELECT * FROM performance_schema.data_lock_waits;
```

#### 1.2.2 排他锁（Exclusive Locks）

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

#### 1.2.3 共享锁（Shared Locks）

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

#### 1.2.4 意向排他锁（Intention Exclusive Locks）

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

#### 1.2.5 意向共享锁（Intention Shared Locks）

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

#### 1.3.1 记录锁（Record Locks）

1. 只有锁定的列为**主键**或**唯一索引**列时，加的才是记录锁
2. 若锁定的列无索引，则会导致整个表被锁住

**当查询列为主键时：**
```sql
# 开启事务
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

# id为主键
mysql> select * from user where id = 1 for update;
+----+----------+-----------+
| id | password | username  |
+----+----------+-----------+
|  1 | rookie   | 120522119 |
+----+----------+-----------+
1 row in set (0.00 sec)

mysql>

# 查询数据库持有的锁
mysql> SELECT * FROM performance_schema.data_locks LIMIT 0, 1000;
+--------+------------------------------------+-----------------------+-----------+----------+---------------+-------------+----------------+-------------------+------------+-----------------------+-----------+---------------+-------------+-----------+
| ENGINE | ENGINE_LOCK_ID                     | ENGINE_TRANSACTION_ID | THREAD_ID | EVENT_ID | OBJECT_SCHEMA | OBJECT_NAME | PARTITION_NAME | SUBPARTITION_NAME | INDEX_NAME | OBJECT_INSTANCE_BEGIN | LOCK_TYPE | LOCK_MODE     | LOCK_STATUS | LOCK_DATA |
+--------+------------------------------------+-----------------------+-----------+----------+---------------+-------------+----------------+-------------------+------------+-----------------------+-----------+---------------+-------------+-----------+
| INNODB | 1162891094680:1121:1162856936152   |                 11615 |        52 |       99 | test          | user        | NULL           | NULL              | NULL       |         1162856936152 | TABLE     | IX            | GRANTED     | NULL      |
| INNODB | 1162891094680:64:4:2:1162856933368 |                 11615 |        52 |       99 | test          | user        | NULL           | NULL              | PRIMARY    |         1162856933368 | RECORD    | X,REC_NOT_GAP | GRANTED     | 1         |
+--------+------------------------------------+-----------------------+-----------+----------+---------------+-------------+----------------+-------------------+------------+-----------------------+-----------+---------------+-------------+-----------+
2 rows in set (0.00 sec)

# 提交事务
mysql> commit;
Query OK, 0 rows affected (0.00 sec)
```

**当查询列为唯一索引时：**
```sql
# 开启事务
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

# password为唯一索引
mysql> select * from user where password='rookie' for update;
+----+----------+-----------+
| id | password | username  |
+----+----------+-----------+
|  1 | rookie   | 120522119 |
+----+----------+-----------+
1 row in set (0.00 sec)

mysql> select * from performance_schema.data_locks;
+--------+------------------------------------+-----------------------+-----------+----------+---------------+-------------+----------------+-------------------+--------------+-----------------------+-----------+---------------+-------------+-----------+
| ENGINE | ENGINE_LOCK_ID                     | ENGINE_TRANSACTION_ID | THREAD_ID | EVENT_ID | OBJECT_SCHEMA | OBJECT_NAME | PARTITION_NAME | SUBPARTITION_NAME | INDEX_NAME   | OBJECT_INSTANCE_BEGIN | LOCK_TYPE | LOCK_MODE     | LOCK_STATUS | LOCK_DATA |
+--------+------------------------------------+-----------------------+-----------+----------+---------------+-------------+----------------+-------------------+--------------+-----------------------+-----------+---------------+-------------+-----------+
| INNODB | 1162891094680:1121:1162856936152   |                 11641 |        52 |      106 | test          | user        | NULL           | NULL              | NULL         |         1162856936152 | TABLE     | IX            | GRANTED     | NULL      |
| INNODB | 1162891094680:64:6:4:1162856933368 |                 11641 |        52 |      106 | test          | user        | NULL           | NULL              | uni_password |         1162856933368 | RECORD    | X,REC_NOT_GAP | GRANTED     | 'rookie'  |
| INNODB | 1162891094680:64:4:2:1162856933712 |                 11641 |        52 |      106 | test          | user        | NULL           | NULL              | PRIMARY      |         1162856933712 | RECORD    | X,REC_NOT_GAP | GRANTED     | 1         |
+--------+------------------------------------+-----------------------+-----------+----------+---------------+-------------+----------------+-------------------+--------------+-----------------------+-----------+---------------+-------------+-----------+
3 rows in set (0.00 sec)
mysql> commit;
Query OK, 0 rows affected (0.00 sec)
```

**当查询列无索引时：**
```sql
# 事务一
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user where password='rookie' for update;
+----+----------+-----------+
| id | password | username  |
+----+----------+-----------+
|  1 | rookie   | 120522119 |
+----+----------+-----------+
1 row in set (0.00 sec)

mysql> select * from performance_schema.data_locks;
+--------+------------------------------------+-----------------------+-----------+----------+---------------+-------------+----------------+-------------------+------------+-----------------------+-----------+-----------+-------------+------------------------+
| ENGINE | ENGINE_LOCK_ID                     | ENGINE_TRANSACTION_ID | THREAD_ID | EVENT_ID | OBJECT_SCHEMA | OBJECT_NAME | PARTITION_NAME | SUBPARTITION_NAME | INDEX_NAME | OBJECT_INSTANCE_BEGIN | LOCK_TYPE | LOCK_MODE | LOCK_STATUS | LOCK_DATA              |
+--------+------------------------------------+-----------------------+-----------+----------+---------------+-------------+----------------+-------------------+------------+-----------------------+-----------+-----------+-------------+------------------------+
| INNODB | 1162891094680:1121:1162856936152   |                 11650 |        52 |      117 | test          | user        | NULL           | NULL              | NULL       |         1162856936152 | TABLE     | IX        | GRANTED     | NULL                   |
| INNODB | 1162891094680:64:4:1:1162856933368 |                 11650 |        52 |      117 | test          | user        | NULL           | NULL              | PRIMARY    |         1162856933368 | RECORD    | X         | GRANTED     | supremum pseudo-record |
| INNODB | 1162891094680:64:4:2:1162856933368 |                 11650 |        52 |      117 | test          | user        | NULL           | NULL              | PRIMARY    |         1162856933368 | RECORD    | X         | GRANTED     | 1                      |
| INNODB | 1162891094680:64:4:3:1162856933368 |                 11650 |        52 |      117 | test          | user        | NULL           | NULL              | PRIMARY    |         1162856933368 | RECORD    | X         | GRANTED     | 2                      |
| INNODB | 1162891094680:64:4:4:1162856933368 |                 11650 |        52 |      117 | test          | user        | NULL           | NULL              | PRIMARY    |         1162856933368 | RECORD    | X         | GRANTED     | 3                      |
| INNODB | 1162891094680:64:4:5:1162856933368 |                 11650 |        52 |      117 | test          | user        | NULL           | NULL              | PRIMARY    |         1162856933368 | RECORD    | X         | GRANTED     | 4                      |
+--------+------------------------------------+-----------------------+-----------+----------+---------------+-------------+----------------+-------------------+------------+-----------------------+-----------+-----------+-------------+------------------------+
6 rows in set (0.00 sec)

# 事务二
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into user values (null,'zhh','zhh');
^C -- query aborted
ERROR 1317 (70100): Query execution was interrupted
```

#### 1.3.2 间隙锁（Gap Locks）

基于`非唯一`索引，它`锁定一段范围内的索引记录`，使用间隙锁锁住的是一个区间，而不仅仅是这个区间的每一条数据。

```sql
# 事务一
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user where age between 2 and 9 for update;
Empty set (0.00 sec)

# 事务二
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user where age = 0 for update;
+----+----------+-----------+-----+
| id | password | username  | age |
+----+----------+-----------+-----+
|  1 | rookie   | 120522119 |   0 |
+----+----------+-----------+-----+
1 row in set (0.00 sec)

mysql> select * from user where age = 20 for update;
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
mysql> select * from user where age = 40 for update;
+----+----------+----------+-----+
| id | password | username | age |
+----+----------+----------+-----+
|  3 | hfg      | hfg      |  40 |
+----+----------+----------+-----+
1 row in set (0.00 sec)
mysql> insert into user values (null,'zn','zn',5);
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
```

由上可知，锁定的区间是`(0,20]`，左开右闭的区间。

#### 1.3.3 临键锁（Next-Key Locks）

`临键锁`存在于`非唯一索引`中，该类型的每条记录的索引上都存在这种锁，它是一种特殊的间隙锁，锁定一段左开右闭的索引区间。

### 1.4 锁问题

通过锁定机制可以实现事务的隔离性要求，使得事务可以并发的工作。锁提高了并发，但是却会带来潜在的问题

#### 1.4.1 隔离级别

`MySQL`支持四种隔离级别，如下表所示，默认支持的是`REPEATABLE READ`。

| 隔离级别 | 描述 |
| :--- | :--- |
| READ UNCOMMITTED | 读未提交 |
| READ COMMITTED | 读提交 |
| REPEATABLE READ | 可重复读 |
| SERIALIZABLE | 串行 |

#### 1.4.2 隔离级别的设置

**查询隔离级别**
```sql
select @@GLOBAL.transaction_isolation;
select @@SESSION.transaction_isolation;
```

**设置隔离级别**
```sql
set GLOBAL TRANSACTION ISOLATION LEVEL REPEATABLE READ;
set SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

#### 1.4.3 脏读

什么是脏读？脏读指的是，A、B两个事务，A事务执行过程中读取到了B事务未提交的数据。

1. 设置隔离级别为`READ UNCOMMITTED`
    ```
    set GLOBAL TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
    ```

2. A事务

    ```
    mysql> begin;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> select * from t_accounts;
    Empty set (0.00 sec)
    ```

3. B事务
    ```
    mysql> begin;
    Query OK, 0 rows affected (0.00 sec)
    
    mysql> insert into t_accounts values (1,1000);
    Query OK, 1 row affected (0.00 sec)
    ```

4. A事务

    ```
    mysql> select * from t_accounts;
    +----+-------+
    | id | money |
    +----+-------+
    |  1 |  1000 |
    +----+-------+
    1 row in set (0.00 sec)
    ```

如上所示：  
在B事务未提交时，A事务就已经查询到了B事务中插入的新数据，这就是脏读

#### 1.4.4 不可重复读
在同一事务中，由于其他事务对数据更改的提交，导致多次查询返回的结果不一致，这就是不可重复读。

1. 设置隔离级别为`READ COMMITTED`
   ```
   set GLOBAL TRANSACTION ISOLATION LEVEL READ COMMITTED;
   ```
2. A事务
   ```
   mysql> begin;
   Query OK, 0 rows affected (0.00 sec)
   
   mysql> select * from t_accounts;
   Empty set (0.00 sec)
   ```

3. B事务
   ```
   mysql> begin;
   Query OK, 0 rows affected (0.00 sec)
   
   mysql> insert into t_accounts values (1, 1000);
   Query OK, 1 row affected (0.00 sec)
   
   mysql> commit;
   Query OK, 0 rows affected (0.01 sec)
   ```

4. A事务
   ```
   mysql> select * from t_accounts;
   +----+-------+
   | id | money |
   +----+-------+
   |  1 |  1000 |
   +----+-------+
   1 row in set (0.00 sec)
   ```

如上所示：  
在A事务未提交过程中，两次查询结果不一致，这就是不可重复读

#### 1.4.5 幻读
           
这里引用看到的博客中的一段话：

*幻读错误的理解：说幻读是 事务A 执行两次 select 操作得到不同的数据集，即 select 1 得到 10 条记录，select 2 得到 11 条记录。这其实并不是幻读，这是不可重复读的一种，只会在 R-U R-C 级别下出现，而在 mysql 默认的 RR 隔离级别是不会出现的。*

*幻读，并不是说两次读取获取的结果集不同，幻读侧重的方面是某一次的 select 操作得到的结果所表征的数据状态无法支撑后续的业务操作。更为具体一些：select 某记录是否存在，不存在，准备插入此记录，但执行 insert 时发现此记录已存在，无法插入，此时就发生了幻读。*

1. 设置隔离级别为`REPEATABLE READ`
```
set GLOBAL TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

2. A事务
```
mysql> begin;
Query OK, 0 rows affected (0.00 sec)
```

3. B事务
```
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into t_accounts values (2,2000);
Query OK, 1 row affected (0.00 sec)

mysql> commit;
Query OK, 0 rows affected (0.01 sec)
```

4. A事务
```
mysql> select * from t_accounts where id = 2;
Empty set (0.00 sec)
mysql> insert into t_accounts values (2,2000);
ERROR 1062 (23000): Duplicate entry '2' for key 'PRIMARY'
```

如上所示：  
A事务执行过程中，B事务插入了主键为2的记录并提交，A这时查询主键为2的记录是查询不到，满足了可重复读，
但是，A事务却无法插入主键为2的记录，看起来没有，但是实际上已经有了，难道我看花了眼？这就是幻读。

#### 1.4.6 隔离级别总结

| | 脏读 | 不可重复读 | 幻读 |
|:--- | :---  | :---- | :--- |
| READ UNCOMMITTED | 会 | 会 | 会 |
| READ COMMITTED | 不会 | 会 | 会 |
| REPEATABLE READ | 不会 | 不会 | 会 |
| SERIALIZABLE | 不会 | 不会 | 不会 |

### 1.5 MVCC


### 1.6 事务的实现方式


### 1.7 死锁

#### 1.7.1 死锁原因
- 两个或者两个以上事务。
- 每个事务都已经持有锁并且申请新的锁。
- 锁资源同时只能被同一个事务持有或者不兼容。
- 事务之间因为持有锁和申请锁导致了循环等待。

#### 1.7.2 死锁的处理方式

MySQL有两种死锁处理方式：

1. 等待，直到超时（innodb_lock_wait_timeo   ut=50s）。
2. 发起死锁检测，主动回滚一条事务，让其他事务继续执行（innodb_deadlock_detect=on）。
由于性能原因，一般都是使用死锁检测来进行处理死锁。

**死锁检测**

死锁检测的原理是构建一个以事务为顶点、锁为边的有向图，判断有向图是否存在环，存在即有死锁。

**回滚**

检测到死锁之后，选择插入更新或者删除的行数最少的事务回滚，基于 INFORMATION_SCHEMA.INNODB_TRX 表中的 trx_weight 字段来判断。
#### 1.7.3 如何避免死锁

网上收集了一些注意事项，如下：
1. 使用事务，不使用 lock tables 。
2. 尽量避免大事务，占有的资源锁越多，越容易出现死锁。建议拆成小事务。
3. 操作完之后立即提交事务，特别是在交互式命令行中。
4. 降低隔离级别。如果业务允许，将隔离级别调低也是较好的选择，比如将隔离级别从 RR 调整为 RC，可以避免掉很多因为 gap 锁造成的死锁。
5. 以固定的顺序访问表和行。交叉访问更容易造成事务等待回路。
6. 为表添加合理的索引。防止没有索引出现表锁，出现死锁的概率会突增。
7. 最好不要用 (SELECT ... FOR UPDATE or SELECT ... LOCK IN SHARE MODE)。


