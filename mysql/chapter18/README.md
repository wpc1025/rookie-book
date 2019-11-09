# Spring事务注意事项

1. 事务可手动进行回滚，代码如下：
```
TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();
```
2. 默认，事务会在发生`RuntimeException`异常及`Error`错误时，会进行事务回滚。通过`rollbackFor`我们可以在某些异常发生时进行回滚。

3. 避免 Spring 的 AOP 的自调用问题

4. `@Transactional`可以注解在接口、类、接口方法、类方法上，建议注解在方法级别上。`@Transactional`注解在方法上时，方法必须是`public`修饰的，否则注解不起作用。
   
5. 查询方法声明不要事务，否则对性能是有影响的。

6. 【参考】 @Transactional 事务不要滥用。事务会影响数据库的 QPS，另外使用事务的地方需要考虑各方面的回滚方案，包括缓存回滚、
搜索引擎回滚、消息补偿、统计修正等。


<hr>

### 问题一：  
项目中，`rollbackFor`设置有两种：`RuntimeException`和`Throwable`，事务默认`RuntimeException`异常是会回滚的，所以没必要设置。是否改成`Throwable`合适？

### 问题二：  
`CertificationChangeRecordServiceImpl.updateAuditStatusByRecordId`方法有两处不需要手动事务回滚，直接返回即可。

### 问题三：
`TransactionMessageServiceImpl.asyncSaveAndCache`的`rollbackFor`为`RuntimeException`异常，改为`Throwable`怎样？该方法为同步保存数据到缓存，异步保存数据到数据库中。
性能与可靠性，你更看重哪一个？

### 问题四：

`VehicleController.createOwner`方法`rollbackFor`为`RuntimeException`，是否改为`Throwable`？

### 问题五：

`VehicleController.activateV1`方法为激活车辆，前面都是查询操作，只有发送影子指令、异步保存影子指令操作为更新操作且有事务控制，是否可以去掉`VehicleController.activateV1`的事务注解？
`VehicleController.activateV2`方法`rollbackFor`为`RuntimeException`，是否改为`Throwable`？

### 问题六：

`VehicleServiceImpl.saveVehicle`方法逻辑为：保存车辆信息、释放分布式锁。问题一：添加分布式锁与释放分布式锁是分离的。问题二：单个的更新sql是否需要事务呢？

### 问题七：

`VehicleOwnerTransferServiceImpl.updateResetResult`，将清除缓存的操作放到事务中
### 问题八：

`VehicleOwnerTransferServiceImpl.resetVehicle`，将清除缓存的操作放到事务中

### 问题九：
`VehicleServiceImpl.updateByVin`加上事务，或者不加事务，先删除缓存，再进行数据库更新的操作呢？






