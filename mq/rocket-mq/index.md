# RocketMQ

[RocketMQ](https://jeasyplus.com/images/mq/rocket-mq.png)

+ [下载安装](https://rocketmq.apache.org/docs/quickStart/01quickstart)

+ 启动：

```shell
$ nohup sh bin/mqnamesrv &

$ tail -f ~/logs/rocketmqlogs/namesrv.log
```
**The Name Server boot success...**

+ 启动Broker和Proxy

```shell
nohup sh bin/mqbroker -n localhost:9876 --enable-proxy &

tail -f ~/logs/rocketmqlogs/proxy.log 
```
The broker[broker-a, 10.0.3.31:10911] boot success. 

rocketmq-proxy startup successfully

一旦在proxy.log中看到“代理[代理名称，IP:端口]启动成功..”的信息，意味着**Broker已成功启动**。

此时，已成功部署了一个单主节点的RocketMQ集群，并且可以通过脚本发送和接收简单的消息。

## 测试
测试前将Nameserver地址设置到系统中，系统环境变量**NAMESRV_ADDR**。

```shell
export NAMESRV_ADDR=localhost:9876
```
注意：外网ip

**生产消息**

```shell
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
```

**接收消息**
```shell
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
```

## Java应用

**集成**
```xml
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-client-java</artifactId>
    <version>5.0.5</version>
</dependency>
```
**创建topic**

创建一个**TestTopic**
```shell
sh bin/mqadmin updatetopic -n localhost:9876 -t TestTopic -c DefaultCluster
```
注意：外网ip

## 停止

```shell
bin/mqshutdown broker

sh bin/mqshutdown namesrv
```

## 基本概念

### 生产者

负责产生消息，RocketMQ提供三种方式发送消息：**同步、异步、单向**。
+ **同步发送：** 收到接收方发回响应之后再发下一个数据包。
+ **异步发送：** 不等接收方发回响应。
+ **单向发送：** 不等待回应且没有回调函数。

### 生产者组
发送同一类消息且发送逻辑一致的 Producer 集合。
多个生产者可以向同一个主题（Topic）或者队列（Queue）发送消息，实现负载均衡，避免单个生产者负载过重。

### 消费者

从消息服务器拉取信息并输出到应用程序，分两种类型：
**拉取型消费者、推送型消费者**。

+ **拉取型消费者（Pull Consumer）：** 应用程序主动拉取到消息。
+ **推送型消费者（Push Consumer）：** 消息到达时调用应用程序的回调接口。

### 消费者组

接收同一类消息且消费逻辑一致的 Consumer 集合。
多个消费者可以共同订阅一个主题（Topic）或者队列（Queue）。以实现消息的负载均衡，避免单个消费者负载过重。

### 消息服务器（Broker）
负责接收、存储和分发消息，并维护消息的持久化存储。
副本管理、消息过滤、消息索引

### 名称服务器（NameServer）
消息队列的元数据管理服务。
维护集群中所有 Broker 和 Topic 的元数据信息，包括 Broker 的地址、Topic 的订阅关系等。
新的 Broker 启动时，它会向 NameServer 注册自己的信息（名称、IP 地址、端口号）。
Producer 和 Consumer 在发送和接收消息时，先向 NameServer 发送路由查询请求，以获取 Topic 对应的 Broker 地址，NameServer提供合适的 Broker 地址，以实现负载均衡。
当 Topic 配置发生变化时（例如增加分区或副本数），NameServer 将及时更新 Broker 的元数据信息。

### 消息
MQ要传输的内容。
一条消息必须有一个主题（Topic），也可以有一个或多个 Tag。

### 主题（Topic）
一种消息分类或类型的概念。
主题可以看作是消息的集合，生产者将消息发送到特定的主题，而消费者则可以订阅感兴趣的主题来接收消息。
主题还可以用于实现消息的路由和过滤。消费者可以根据主题来过滤不需要的消息。

### 标签
对消息更细粒度的分类。
允许生产者发送消息时为每条消息附加一个或多个标签，消费者可以根据标签来选择性地订阅和消费消息。
标签可以实现消息的多路复用，提高消息的灵活性和可扩展性。

### 消息队列
消息队列是一个逻辑上的容器，一个主题下可以有多个队列。
消息队列根据主题和路由策略将消息存储到相应的队列中。
消息队列是用于存储和传递消息的逻辑概念。
负责将生产者发送的消息存储到相应的主题队列中，并根据消费者的订阅情况将消息发送给相应的消费者。

### 消费模式
+ 集群消费模式（Clustered Consumer）:
  + **一个队列**只会被**一个消费者**消费。**同一组消费者**共同消费一个**主题（Topic）下的多个队列**。

+ 广播消费模式（Broadcasting Consumer）
  消息会发给消费组的每一个消费者。

### 消息顺序

+ 顺序消费（Orderly）
+ 并行消费（Concurrently）

### 消息分类
+ 延时消息
+ 事务消息
+ 顺序消息

### 消息过滤

+ Tag 过滤
```java
consumer.subscribe("TopicTest", "TagA || TagB");
```
+ SQL92 过滤。
```java
  consumer.subscribe("TopicTest", MessageFilter.get(
                FilterFactory.INSTANCE.get(MessageFilter.class).compile("Tag is not null", ExpressionType.SQL92)
   ));

// SELECT * FROM TopicTest WHERE property1 = 'value1'
```

### 死信队列

用于存储无法被正常消费的消息
```java
 // 设置消息的最大重试次数和重试间隔时间（单位：毫秒）
 message.setRetryTimesWhenSendFailed(3);
 message.setRetryAnotherBrokerInterval(1000);
```
消费者订阅死信队列主题，处理无法被正常消费的消息。
```java
// 订阅死信队列主题
consumer.subscribe("DLQTopic", "*");

consumer.registerMessageListener(new MessageListenerConcurrently() {
    @Override
    public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt> msgs, ConsumeConcurrentlyContext context) {
        for (MessageExt message : msgs) {
            // 处理死信队列中的消息
            System.out.println("Received dead letter message: " + new String(message.getBody()));
        }
        return ConsumeConcurrentlyStatus.CONSUME_SUCCESS;
    }
});

consumer.start();
System.out.println("Dead letter consumer started.");
```

### Consumer端重试
+ 顺序重试：
  + 将消息返回到消息队列并暂停消费一段时间（由消费者配置的 suspendCurrentQueueTimeMillis 参数决定），然后再次尝试消费该消息。

```java
DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("ConsumerGroup");
consumer.setNamesrvAddr("localhost:9876");

// 设置消息顺序重试的暂停时间（单位：毫秒）
consumer.setSuspendCurrentQueueTimeMillis(5000);

consumer.subscribe("TopicTest", "*");

consumer.registerMessageListener(new MessageListenerConcurrently() {
    
});
```

+ 无序重试
  + 消费者可以选择立即重新消费消息，而不是等待一段时间。

```java
DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("ConsumerGroup");
consumer.setNamesrvAddr("localhost:9876");

// 设置消息重试次数，默认为 16次
consumer.setMaxReconsumeTimes(3);

consumer.subscribe("TopicTest", "*");

consumer.registerMessageListener(new MessageListenerConcurrently() {
    // todo    
});
```

### 刷盘方式

+ 同步刷盘
  + 数据到达内存之后，必须刷到CommitLog日志,才会给producer返回发送成功。
+ 异步刷盘
  + 数据到达内存之后,producer发送成功。，然后写入CommitLog日志。

### ConsumeQueue

一个物理文件，每个消息队列都对应一个 ConsumeQueue 文件。

用于存储该消息队列上的消息消费进度，消费者成功消费了一条消息，ConsumeQueue 会记录该消息的消费进度，后续跳过已消费的消息。

RocketMQ 的消息存储结构如下：
+ 主题（Topic）包含多个消息队列（Message Queue）。


+ 每个消息队列包含一个 ConsumeQueue 文件和多个消息存储文件（CommitLog）。


+ 每个消息存储文件包含多条消息。

### CommitLog

持久化存储生产者发送的消息。

CommitLog 是一个顺序写入的文件，记录生产者发送的消息内容，每条消息都会追加到 CommitLog 文件的末尾。

















