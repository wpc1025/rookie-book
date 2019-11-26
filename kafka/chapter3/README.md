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





