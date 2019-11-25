# 一、初始Kafka

- LinkedIn公司开发，采用Scala语言，多分区、多副本、基于Zookeeper协调的分布式消息系统，目前已经捐赠给Apache
- 分布式流处理平台，以高吞吐、可持久化、可水平扩展、支持流数据处理等多种特性而被广泛使用
- 三大角色
    - 消息系统：提供解耦、冗余存储、流量削峰、缓冲、异步通信、扩展性、可恢复性、消息顺序性保障、回溯消费等功能
    - 存储系统
    - 流式处理平台
    
## 1.1 基本概念

- Producer：生产者，负责创建消息，然后将其投递到Kafka中
- Consumer：消费者，消费者连接到Kafka上并接收消息，进而进行相应的业务逻辑处理。使用拉（PULL）模式从服务端拉取消息，并保存消费的具体位置
- Broker：服务代理节点
- Topic：Kafka中的消息以主题进行分类
- Partition：主题是一个逻辑概念，可以细分为多个分区，一个分区只属于单个主题，同一主题不同分区包含的消息是不同的，分区在存储层面可以看做一个可追加的日志文件
- Offset：消息在分区中的唯一标识，通过它保证分区有序，但不能保证主题有序
- Replica：Kafka为分区引入了多副本机制，该机制实现了故障自动转移。一主多从，leader副本负责处理读写请求，follow副本只负责与leader副本的消息同步。
所有副本统称AR（Assigned Replicas），与Leader副本保持一定程度同步的副本（包括Leader副本）组成ISR（In-Sync Replicas），与Leader副本同步滞后过多的副本
（不包括Leader副本）组成OSR（Out-of-Sync Replicas）
- HW：High Watermark缩写，高水位，标识一个特定的offset，消费者只能拉取到这个offset之前的消息
- LEO：Log End Offset缩写，表示当前日志文件中下一条待写入消息的offset，ISR集合中最小的LEO即为分区的HW

![Kafka体系结构](./1-1.png)

如下：LEO为5，HW为4

![写入消息](./1-2.png)

## 1.2 安装和配置

安装步骤省略

一些常见的kafka命令

- 启动kafka
```
bin/kafka-server-start.sh config/server.properties
bin/kafka-server-start.sh -daemon config/server.properties
bin/kafka-server-start.sh config/server.properties &
```

- 创建主题
```
bin/kafka-topics.sh --zookeeper localhost:2181/kafka --create --topic topic-demo --replication-factor 3 --partitions 4
```

- 获取主题的详细信息
```
bin/kafka-topic.sh --zookeeper localhost:2181/kafka --describe --topic topic-demo
```

- 命令行消费消息
```
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic topic-demo
```

- 命令行发送消息
```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic topic-demo
```

## 1.3 java客户端

**生产者Java端代码**

```java
package com.mrrookie.practice.producer;

import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerConfig;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.common.serialization.StringSerializer;

import java.util.Properties;

/**
 * Kafka生产者客户端
 */
public class ProducerFastStart {
    public static final String brokerList = "localhost:9092";
    public static final String topic = "topic-demo";

    public static void main(String[] args) {
        Properties properties = new Properties();
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, brokerList);

        KafkaProducer<String, String> producer = new KafkaProducer<String, String>(properties);
        ProducerRecord<String, String> record = new ProducerRecord<>(topic, "hello, Kafka");

        try {
            producer.send(record);
        } catch (Exception e) {
            e.printStackTrace();
        }

        producer.close();
    }
}
```

**消费者Java端代码**

```java
package com.mrrookie.practice.consumer;

import org.apache.kafka.clients.consumer.ConsumerConfig;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.common.serialization.StringDeserializer;

import java.time.Duration;
import java.util.Collections;
import java.util.Properties;

/**
 * Kafka消费端实例
 */
public class CosumerFastStart {
    public static final String brokerList = "localhost:9092";
    public static final String topic = "topic-demo";
    public static final String groupId = "group.demo";

    public static void main(String[] args) {
        Properties properties = new Properties();
        properties.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, brokerList);
        properties.put(ConsumerConfig.GROUP_ID_CONFIG, groupId);

        KafkaConsumer<String, String> consumer = new KafkaConsumer<String, String>(properties);
        consumer.subscribe(Collections.singletonList(topic));

        while (true) {
            ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(1000));
            for (ConsumerRecord<String, String> record : records) {
                System.out.println(record.value());
            }
        }
    }
}
```

## 1.4 服务端参数配置

| 参数 | 描述 |
| :--- | :--- |
| zookeeper.connect | 指明broker要连接的Zookeeper集群的服务地址（包含端口号），没有默认值，必填。建议加一个chroot路径，如 localhost:2181,localhost2:2181,localhost3:2181/kafka |
| listeners | 指明broker监听客户端连接的地址列表，即为客户端要连接的broker的入口地址列表，配置格式为 protocol1://hostname1:port1,protocol2:hostname2:port2，其中protocol代表协议类型，支持PLAINTEXT、SSL、SASL_SSL等 |
| broker.id | 指定kafka集群中broker的唯一标识，默认值是-1，如果没有设置，Kafka会自动生成一个 |
| log.dir 和 log.dirs | 配置Kafka日志文件存放的根目录 |
| message.max.bytes | 指定broker所能接收消息的最大值，默认值是1000012B。Producer发送的消息大小超过该值会报RecordTooLargeException异常。不建议修改，建议分拆消息 |




