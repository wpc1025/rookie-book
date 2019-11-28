# 三、消费者

通过KafkaConsumer订阅主题，并从订阅的主题中拉取消息

## 3.1 消费者和消费者组

- 每个消费者都有一个对应的消费者组，当消息发布到主题之后，只会被投递给订阅它的每个消费组中的一个消费者
- 消费者与消费者组可以让整体的消费能力具备横向伸缩性，可以增加或减少消费者的个数来提高（或降低）整体的消费能力
- 消费者个数大于分区个数的情况，会出现消费者分配不到任何分区
- 可通过消费者客户端参数partition.assignment.strategy来设置消费者和订阅主题之间的分区分配策略
- Kafka支持点对点模式和发布/订阅模式
- 消费者可通过指定group.id参数来设置所属消费者组的名称

## 3.2 客户端开发

**正常的消费逻辑需要具备以下几个步骤**
- 配置消费者客户端参数及创建响应的消费者实例
- 订阅主题
- 拉取消息并消费
- 提交消费位移
- 关闭消费者实例

### 3.2.1 必要的参数设置

| 参数 | 描述 |
| :--- | :--- |
| bootstrap.servers | 必填，用来指定消费者客户端连接Kafka集群所需的broker地址清单，格式为 host1:port1,host2:port2,host3:port3 |
| group.id | 消费者隶属的消费组的名称，默认为""，若不设置，则会抛异常 |
| key.deserializer 与 value.desrializer | 指定反序列化器，与生产者对应 |
| client.id | 设置KafkaConsumer对应的客户端ID，默认为""，若客户端不设置，则自动生成一个非空字符串 |

### 3.2.2 订阅主题和分区

**订阅主题的方式**
- 集合订阅主题列表：
```
KafkaConsumer.subscribe(Collection<String> topics)
KafkaConsumer.subscribe(Collection<String> topics, ConsumerRebalanceListener listener)
```
- 正则表达式订阅主题：
```
KafkaConsumer.subscribe(Pattern pattern)
KafkaConsumer.subscribe(Pattern pattern, ConsumerRebalanceListener listener)
```
- 订阅指定主题的指定分区的消息
```
KafkaConsumer.assign(Collection<TopicPartition> partitions)
```

**取消订阅的方式如下**
```
KafkaConsumer.unsubscribe()
KafkaConsumer.subscribe(new ArraysList<String>())
KafkaConsumer.assign(new ArraysList<TopicPartition>())
```

通过subscribe()方法订阅主题具备消费者自动再均衡的功能，在多个消费者的情况下可以根据分区策略自动分配消费者和分区的关系。但assign()方法不具备自动再
均衡的能力

### 3.2.3 反序列化

与生产者的序列化器类似，Kafka自身提供了一些反序列化器，也支持自定义序列化器，并支持Avro、JSON等通用序列化器。

### 3.2.4 消息消费

- Kafka中的消费基于拉模式
- 通过`public ConsumeRecords<K,V> poll(final Duration timeout)`拉取消息，timeout控制poll方法的阻塞时间
- ConsumeRecords类提供了一些有用的方法，包括消息集合中包含了哪几个分区的消息，消息的总数量，消息集合是否为空等方法。

### 3.2.5 位移提交

- 消费者的位移需要持久化保存，原因有二：防止消费者重启之后不知道之前的消费位移；新的消费者加入的时候，会进行再均衡动作，若不持久化保存消费位移，新的消费者
就无法知道之前的消费位移。消费位移存储在Kafka内部的主题__consumer_offset中
- 消费者提交的位移为下一次拉取的消息offset，如当前已经消费到的offset为x，则提交时提交的其实是x+1
- 消费位移提交的时机也很有讲究，否则，有可能会造成重复消费和消息丢失的现象
- Kafka中默认的消息位移提交方式是自动提交，由客户端参数 enable.auto.commit 配置，默认为true。定期提交，默认5秒，在向服务端发起拉取请求的时候会检查是否
可以进行位移提交，如果可以，就会提交上一次轮询的位移。
- Kafka支持手动提交的方式，设置客户端参数 enable.auto.commit 为false开启。手动提交细分为同步提交和异步提交。
```
KafkaConsumer方法

// 同步提交方式
public void commitSync();
public void commitSync(final Map<TopicPartition, OffsetAndMetadata> offsets);

// 异步提交方式
public void commitAsync();
public void commitAsync(OffsetCommitCallback callback);
public void commitAsync(final Map<TopicPartition, OffsetAndMetadata> offsets, offsetCommitCallback callback);
```

### 3.2.6 控制或关闭消费

- KafkaConsumer中使用 pause() 和 resume() 方法来分别实现暂停某些分区在拉取操作时返回数据给客户端和恢复某些分区向客户端返回数据的操作
```
public void pause(Collection<TopicPartition> partitions);
public void resume(Collection<TopicPartition> partitions);

// 返回被暂停的分区集合
public Set<TopicPartition> pause();
```
- 如何优雅的推出拉取消息的循环？一种是使用 while(isRunning.get()) 的方式，还有一种是调用 KafkaConsumer 的 wakeup() 方法
- 跳出循环之后要显式的执行关闭动作以释放运行过程中占用的各种系统资源，使用 KafkaConsumer 的 close() 方法
```
public void close();
public void close(Duration timeout);
public void close(long timeout, TimeUnit timeunit);
```

### 3.2.7 指定位移消费

- 在Kafka中，当消费者查找不到所记录的消费位移或位移越界时，就会根据消费者客户端设置的参数 auto.offset.reset 的配置来决定从何处进行消费
    - latest，默认值，从分区末尾开始消费
    - earliest 从起始处开始消费
    - none 报出 NoOffsetForPartitionException 异常
    - 不配置，报出 ConfigException 异常
- KafkaConsumer 提供的 seek() 方法，可以让我们从特定的位移处开始拉取消息，得以追前消费或回溯消费
```
public void seek(TopicPartition partition, long offset)
```
- KafkaConsumer 直接提供了 seekToBeginning() 方法和 seekToEnd() 方法来从分区的开头和末尾开始消费
- KafkaConsumer 提供了 offsetsForTimes() 方法，通过 timestamp 来查询与此对应的分区位置

### 3.2.8 再均衡

- 指分区的所属权从一个消费者转移到另一个消费者的行为，为消费组具备高可用性和伸缩性提供保障
- 在再均衡发生期间，消费组内的消费者无法读取消息。且有可能发生重复消费
- KafkaConsumer 的 subscribe() 方法提供了再均衡监听器 ConsumerRebalanceListener 接口，提供了两个方法。
```
// 在再均衡发生之前和消费者停止读取消息之后被调用
void onPartitionsRevoked(Collection<TopicPartition> partitions)

// 在重新分配分区之后和消费者开始读取消费之前被调用
void onPartitionsAssigned(Collection<TopicPartition> partitions)
```

### 3.2.9 消费者拦截器

- 消费者拦截器主要在消费到消息或在提交消费位移时进行一些定制化的操作
- 需要自定义实现 org.apache.kafka.clients.consumer.ConsumerInterceptor 接口
```
// 在poll()方法返回之前调用拦截器的 onConsume 方法对消息进行相应的定制化操作
public ConsumerRecords<K,V> onConsume(ConsumerRecords<K,V> records)

// KafkaConsumer 会在提交完消费位移之后调用拦截器的 onCommit() 方法，可以使用这个方法跟踪所提交的位移信息
public void onCommit(Map<TopicPartition, OffsetAndMetadata> offset)

public void close()
```
- 自定义的消费者拦截器，需要在 KafkaConsumer 中配置这个拦截器
- 同样，可配置多个拦截器，组成拦截器链

### 3.2.10 多线程实现

- KafkaConsumer 是非线程安全的，定义了一个 acquire() 方法，用来检测当前是否只有一个线程在操作，若有其他线程正在操作则会抛出 ConcurrentModifcationException 异常
- 多线程的实现方式
    - 线程封闭，每个线程都实例化一个 KafkaConsumer 对象。并发度受限于分区的实际个数
    - 多个消费线程同时消费同一个分区。打破消费线程个数不能超过分区数的限制，不过对于位移提交和顺序控制的处理就会变得复杂
    - 将处理消息模块改成多线程的实现方式（推荐），要注意消息的顺序消费及位移提交的时机，有可能会造成消息丢失。
    
### 3.2.11 消费者参数

| 参数 | 描述 |
| :--- | :--- |
| fetch.min.bytes | 配置Consumer在一次拉取请求（调用poll方法）中能从kafka中拉取的最小数据量，默认值为1B |
| fetch.max.bytes | 配置Consumer在一次拉取请求（调用poll方法）中能从kafka中拉取的最大数据量，默认值为50MB |
| fetch.max.wait.ms | 指定Kafka的等待时间，默认500ms，超过这个时间，不管是否满足 fetch.min.bytes 的要求，都会返回 |
| max.partition.fetch.bytes | 用来配置从每个分区里返回给 consumer 的最大数据量，默认是1MB |
| max.poll.records | 用来配置 Consumer 在一次拉取请求中拉取的最大消息数，默认值为500条 |
| exclude.internal.topics | 用来指定Kafka中的内部主题是否可以向消费者公开，默认为true |
    



