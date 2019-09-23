
## MySQL事务

1. `MySQL`引擎为`Innodb`时才支持事务
2. `MySQL`的`Innodb`引擎支持四种隔离级别，分别为`READ UNCOMMITTED`、`READ COMMITTED`、`REPEATABLE READ`、`SERIALIZABLE`，默认为`REPEATABLE READ`。
这四种隔离级别可能存在的问题如下：

    | | 脏读 | 不可重复读 | 幻读 |
    | :--- | :--- | :--- | :--- |
    | READ UNCOMMITTED | 会 | 会 | 会 |
    | READ COMMITTED | 不会 | 会 | 会 |
    | REPEATABLE READ | 不会 | 不会 | 会 |
    | SERIALIZABLE | 不会 | 不会 | 不会 |

## Spring 事务

1. Spring事务可配置属性及默认值如下：

    | 属性 | 默认 | 描述 |
    | :--- | :--- | :--- |
    | propagation | REQUIRED | 事务传播行为 |
    | isolation | DEFAULT | 事务隔离级别，默认使用数据库的隔离级别设置 |
    | timeout | -1 | 事务超时（秒），仅适用于`REQUIRED`或`REQUIRES_NEW` |
    | read-only | false | `Read-write`或`read-only`事务，，仅适用于`REQUIRED`或`REQUIRES_NEW` |
    | rollback-for | | 逗号分隔的Exception触发回滚的实例列表 |
    | no-rollback-for | | 逗号分隔的Exception实例列表，不触发回滚 |
    
    - `propagation`：事务传播行为
        - PROPAGATION_REQUIRED ：如果存在一个事务，则支持当前事务。如果没有事务则开启一个新的事务。 
        - PROPAGATION_SUPPORTS ： 如果存在一个事务，支持当前事务。如果没有事务，则非事务的执行。
        - PROPAGATION_MANDATORY ： 如果已经存在一个事务，支持当前事务。如果没有一个活动的事务，则抛出异常。
        - PROPAGATION_REQUIRES_NEW ： 它会开启一个新的事务。如果一个事务已经存在，则先将这个存在的事务挂起。
        - PROPAGATION_NOT_SUPPORTED ： 总是非事务地执行，并挂起任何存在的事务。
        - PROPAGATION_NEVER ： 总是非事务地执行，如果存在一个活动事务，则抛出异常。
        - PROPAGATION_NESTED ： 如果一个活动的事务存在，则运行在一个嵌套的事务中。 如果没有活动事务, 则按`PROPAGATION_REQUIRED`属性执行。 

2. Spring事务默认的事务规则是遇到运行异常（RuntimeException及其子类）和程序错误（Error）才会进行事务回滚。
3. `@Transactional`注解可应用于接口、接口方法、类、类公共方法。建议`@Transactional`注解应用于类公共方法上，而不是接口、接口方法、类上。
4. Spring事务，在代理模式下，仅拦截通过代理传入的外部方法调用，自调用（目标对象内的方法调用目标对象的另一个方法）时，不会在运行时使用被调用方法上的事务配置。
5. 编程事务管理的方式：
    - `TransactionTemplate`
    - `PlatformTransactionManager`
6. 当要求一个事务提交之后触发事件的处理时，使用`@TransactionalEventListener`
7. 除通过`@Transactional`注解的`rollbackFor`属性配置发生何种异常时进行事务回滚，也可以通过编程式方式进行回滚，如下：
    `TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();`

