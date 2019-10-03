# MySQL中的锁与事务

## 一、锁

我们都知道`MySQL`的存储引擎是插件式的，支持`MyISAM`、`InnoDB`、`NDB`、`Memory`等存储引擎。不同的存储引擎支持不同的锁级别，比如，`MyISAM`支持表锁，
`Memory`支持表锁，`InnoDB`支持行锁。以下默认介绍的都是`InnoDB`存储引擎中的锁。

### 1.1 锁及其意义

锁是数据库系统区别于文件系统的一个关键特性。锁机制用于管理对共享资源的并发访问。

数据库使用锁是为了支持对共享资源进行并发访问，提供数据的完整性和一致性。

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

#### 1.2.5 记录锁（Record Locks）


