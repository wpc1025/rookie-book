# 事务

- Innodb引擎才支持事务
- MySql默认隔离级别为`REPEATABLE READ`

## 一、隔离级别的设置

**查询隔离级别**
```
select @@GLOBAL.transaction_isolation;
select @@SESSION.transaction_isolation;
```
**设置隔离级别**
```
set GLOBAL TRANSACTION ISOLATION LEVEL REPEATABLE READ;
set SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

**隔离级别分为以下四种**

| 隔离级别 | 描述 |
| :--- | :--- |
| READ UNCOMMITTED | 读未提交 |
| READ COMMITTED | 读提交 |
| REPEATABLE READ | 可重复读 |
| SERIALIZABLE | 串行 |

## 二、脏读、不可重复读、幻读

创建一张表，名为`t_accounts`

```
CREATE TABLE `t_accounts` (
  `id` int(11) NOT NULL,
  `money` int(11) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB;
```

### 2.1 脏读

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

### 2.2 不可重复读
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

### 2.3 幻读

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

## 三、隔离级别总结

| | 脏读 | 不可重复读 | 幻读 |
|:--- | :---  | :---- | :--- |
| READ UNCOMMITTED | 会 | 会 | 会 |
| READ COMMITTED | 不会 | 会 | 会 |
| REPEATABLE READ | 不会 | 不会 | 会 |
| SERIALIZABLE | 不会 | 不会 | 不会 |
