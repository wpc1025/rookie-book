# Spring事务

The Spring team recommends that you annotate only concrete classes (and methods of concrete classes) with the @Transactional annotation, as opposed to annotating interfaces. You certainly can place the @Transactional annotation on an interface (or an interface method), but this works only as you would expect it to if you use interface-based proxies. The fact that Java annotations are not inherited from interfaces means that, if you use class-based proxies (proxy-target-class="true") or the weaving-based aspect (mode="aspectj"), the transaction settings are not recognized by the proxying and weaving infrastructure, and the object is not wrapped in a transactional proxy.

In proxy mode (which is the default), only external method calls coming in through the proxy are intercepted. This means that self-invocation (in effect, a method within the target object calling another method of the target object) does not lead to an actual transaction at runtime even if the invoked method is marked with @Transactional. Also, the proxy must be fully initialized to provide the expected behavior, so you should not rely on this feature in your initialization code (that is, @PostConstruct).

方法级别`@Transactional`注解优于类级别`@Transactional`注解配置，不建议在类级别设置`@Transaction`注解。

`PROPAGATION_REQUIRED`
如果内部事务（外部调用者不知道）将事务无提示地标记为仅回滚，则外部调用者仍会调用commit。外部调用者需要接收一个UnexpectedRollbackException，以清楚地指示已执行回滚。





