# RabbitMQ


RabbitMQ 是一个实现了AMQP（Advanced Message Queuing Protocol）协议的消息队列中间件。

用于在分布式系统中进行消息传递。

RabbitMQ 的架构采用了一种经典的消息代理模型，它由以下几个核心组件组成：
+ Producer（生产者）
+ Exchange（交换机）
+ Queue（消息队列）
+ Binding（绑定）
+ Virtual Host（虚拟主机）

## 基本原则：

生产端的代码差异取决于**Exchange（交换机）**

+ 直接交换机（Direct Exchange）
+ 主题交换机（Topic Exchange）
+ 扇形交换机（Fanout Exchange）
+ 头部交换机（Headers Exchange）
+ 系统默认交换机（Default Exchange）


消费端（Consumer）的代码差异取决于**消息模式**
+ 简单模式（Simple Mode）
+ 工作队列模式（Work Queue Mode）
+ 发布-订阅模式（Publish-Subscribe Mode）
+ 路由模式（Routing Mode）
+ 主题模式（Topic Mode）
+ 头部模式（Headers Mode）

消息路由差异取决于**Binding（绑定）**

## RabbitMQ消息模式

### Producer（生产者）：

消息的发送方。

负责产生并发送消息到 RabbitMQ 的交换机（Exchange）。

并指定消息的路由键（Routing Key）来决定消息的路由方式。

### Exchange（交换机）：

消息的路由中心。

接收来自生产者的消息，并根据消息的**路由键**将消息**路由**到相应的**消息队列**。

**最终的路由**是由消息的**路由键**和**配置的绑定（Binding）规则**决定。

### Queue（消息队列）：

消息的存储区

保存经过交换机路由过来的消息。

消费者从消息队列中订阅并消费消息。

消息队列支持先进先出（FIFO）的消息传递方式，确保消息按照顺序进行消费。

### Binding（绑定）

是 Exchange 和 Queue 之间的关联规则。

定义了消息从交换机路由到哪些队列中。

Binding 将交换机和队列关联起来，使得消息可以正确地路由到目标队列。

### Consumer（消费者）：

消息的接收方。

负责从消息队列中订阅并消费消息。

消费者从队列中获取消息并处理，确保消息得到正确地消费。

### Virtual Host（虚拟主机）：

虚拟主机是 RabbitMQ 中的逻辑概念。

用于实现逻辑隔离。

每个虚拟主机拥有自己独立的交换机、队列、绑定等资源，确保不同应用之间的消息隔离和安全性。

## 示例代码

### 简单模式（Simple Mode）：

也称**点对点模式**，是最基本的消息模式。

消息被直接发送到队列，被一个消费者接收并消费。

**生产者**
```java
// 创建通道
Channel channel = connection.createChannel()) {
// 声明队列
channel.queueDeclare(QUEUE_NAME, false, false, false, null);

// 要发送的消息
String message = "Hello RabbitMQ!";

// 发送消息到队列
channel.basicPublish("", QUEUE_NAME, null, message.getBytes());
```
**消费者**
```java
// 创建通道
Channel channel = connection.createChannel()) {
// 声明队列
channel.queueDeclare(QUEUE_NAME, false, false, false, null);

// 创建消费者
DeliverCallback deliverCallback = (consumerTag, delivery) -> {
    String message = new String(delivery.getBody(), "UTF-8");
    System.out.println(" [x] Received '" + message + "'");
};

// 消费消息
channel.basicConsume(QUEUE_NAME, true, deliverCallback, consumerTag -> { });
```

### 工作队列模式（Work Queue Mode）

也称为任务队列模式。

多个消费者共享一个队列，消息被平均分配给不同的消费者。

实现任务的并行处理，提高系统的处理能力。

**生产者**
```java
// 创建通道
 Channel channel = connection.createChannel()) {
// 声明队列
channel.queueDeclare(QUEUE_NAME, false, false, false, null);

// 要发送的消息
String message = "Hello RabbitMQ!";

// 发送消息到队列
for (int i = 0; i < 5; i++) {
    channel.basicPublish("", QUEUE_NAME, null, (message + " " + i).getBytes());
    System.out.println(" [x] Sent '" + message + " " + i + "'");
}
```

**消费者**
```java
// 设置每次从队列中获取的消息数量
int prefetchCount = 1;
channel.basicQos(prefetchCount);

// 创建消费者
DeliverCallback deliverCallback = (consumerTag, delivery) -> {
String message = new String(delivery.getBody(), "UTF-8");
System.out.println(" [x] Received '" + message + "'");

// 模拟消息处理耗时
try {
    Thread.sleep(2000);
} catch (InterruptedException e) {
    e.printStackTrace();
}

// 手动发送确认消息
channel.basicAck(delivery.getEnvelope().getDeliveryTag(), false);
```

### 发布-订阅模式（Publish-Subscribe Mode）：

也称为广播模式。

该模式下消息会被发送到交换机（Exchange），然后被绑定到交换机的多个队列中。

每个队列都有一个消费者，当消息被发送到交换机时，所有绑定的队列都会收到该消息的副本，实现消息的广播传递。

#### 交换类型

+ 直接交换机（Direct Exchange）：
  + 根据消息的路由键（Routing Key），将消息路由到与之完全匹配的队列。
+ 主题交换机（Topic Exchange）：
  + 根据消息的路由键和通配符模式，将消息路由到与之匹配的队列，
+ 扇形交换机（Fanout Exchange）：
  + 将消息广播到所有与之绑定的队列，无论消息的路由键。
+ 头部交换机（Headers Exchange）：
  + 根据消息的头部属性来进行匹配和路由，不关心消息的路由键。
+ 系统默认交换机（Default Exchange）：
  + 一个直接交换机，没有名称，将队列与系统默认交换机绑定，使用队列名称作为路由键，即可实现消息的路由和传递。

#### 直接交换机（Direct Exchange）：
```java
// 创建直接交换机
channel.exchangeDeclare("direct_exchange", BuiltinExchangeType.DIRECT);

// 发送消息到直接交换机，使用指定的路由键
channel.basicPublish("direct_exchange", "routing_key", null, message.getBytes());
```

#### 主题交换机（Topic Exchange）：

```java
// 创建主题交换机
channel.exchangeDeclare("topic_exchange", BuiltinExchangeType.TOPIC);

// 发送消息到主题交换机，使用指定的路由键
channel.basicPublish("topic_exchange", "order.create", null, message.getBytes());
```

#### 扇形交换机（Fanout Exchange）：

```java
// 创建扇形交换机
channel.exchangeDeclare("fanout_exchange", BuiltinExchangeType.FANOUT);

// 发送消息到扇形交换机，不需要指定路由键
channel.basicPublish("fanout_exchange", "", null, message.getBytes());
```

#### 头部交换机（Headers Exchange）：

```java
// 创建头部交换机
channel.exchangeDeclare("headers_exchange", BuiltinExchangeType.HEADERS);

// 设置消息的头部属性
Map<String, Object> headers = new HashMap<>();
headers.put("key1", "value1");
headers.put("key2", "value2");

// 发送消息到头部交换机，使用指定的头部属性
AMQP.BasicProperties properties = new AMQP.BasicProperties.Builder()
        .headers(headers)
        .build();

channel.basicPublish("headers_exchange", "", properties, message.getBytes());
```

#### 系统默认交换机（Default Exchange）：

```java
// 不需要创建交换机，使用默认交换机将消息路由到指定队列
channel.basicPublish("", "queue_name", null, message.getBytes());
```

### 路由模式（Routing Mode）

使用路由键通过交换机（Exchange）将消息路由到具有队列。

每个消费者可以选择订阅特定的路由键。只接收感兴趣的消息。

**生产者**
```java
// 创建通道
Channel channel = connection.createChannel()) {

// 声明直接交换机，类型为Direct
channel.exchangeDeclare(EXCHANGE_NAME, "direct");

// 要发送的消息
String message = "Hello RabbitMQ Routing Mode!";

// 发送消息到交换机，并指定路由键为"my_routing_key"
channel.basicPublish(EXCHANGE_NAME, ROUTING_KEY, null, message.getBytes());
```
**消费者**
```java
// 创建通道
Channel channel = connection.createChannel()) {

// 声明直接交换机，类型为Direct
channel.exchangeDeclare(EXCHANGE_NAME, "direct");

// 声明队列
channel.queueDeclare(QUEUE_NAME, false, false, false, null);

// 将队列绑定到交换机，并指定路由键为"my_routing_key"
channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, ROUTING_KEY);

// 创建消费者
DeliverCallback deliverCallback = (consumerTag, delivery) -> {
  String message = new String(delivery.getBody(), "UTF-8");
  System.out.println(" [x] Received '" + message + "' with routing key '" + delivery.getEnvelope().getRoutingKey() + "'");
};

// 消费消息
channel.basicConsume(QUEUE_NAME, true, deliverCallback, consumerTag -> {
});
```

### 主题模式（Topic Mode）：

也称为通配符模式，是路由模式的扩展，支持通配符匹配。

可以根据模式匹配消息的路由键来实现更灵活的消息过滤和路由。

#### 主题模式中的通配符有两种：

+ *（星号）：用于匹配一个单词（单个单词或一个单词）。
+ #（井号）：用于匹配零个或多个单词。

**生产者**

```java
// 创建通道
Channel channel = connection.createChannel()) {

// 声明主题交换机，类型为Topic
channel.exchangeDeclare(EXCHANGE_NAME, "topic");

// 要发送的消息
String message = "Hello RabbitMQ Topic Mode!";

// 发送消息到交换机，并指定路由键为"order.create"
channel.basicPublish(EXCHANGE_NAME, ROUTING_KEY, null, message.getBytes());
```

**消费者**

```java
// 创建通道
Channel channel = connection.createChannel()) {

// 声明主题交换机，类型为Topic
channel.exchangeDeclare(EXCHANGE_NAME, "topic");

// 声明队列
channel.queueDeclare(QUEUE_NAME, false, false, false, null);

// 将队列绑定到交换机，并指定绑定键为"order.#"
channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, BINDING_KEY);

// 创建消费者
DeliverCallback deliverCallback = (consumerTag, delivery) -> {
    String message = new String(delivery.getBody(), "UTF-8");
    System.out.println(" [x] Received '" + message + "' with routing key '" + delivery.getEnvelope().getRoutingKey() + "'");
};

// 消费消息
channel.basicConsume(QUEUE_NAME, true, deliverCallback, consumerTag -> {
});
```

### 头部模式（Headers Mode）：

消息的路由规则不再依赖于路由键，而由消息头部的属性来决定。

这样可以实现更复杂的消息匹配和路由方式。

**生产者**
```java
// 创建通道
Channel channel = connection.createChannel()) {

// 声明头部交换机，类型为Headers
channel.exchangeDeclare(EXCHANGE_NAME, "headers");

// 要发送的消息
String message = "Hello RabbitMQ Headers Mode!";

// 设置消息的头部属性
Map<String, Object> headers = new HashMap<>();
headers.put("header1", "value1");
headers.put("header2", "value2");

// 创建消息的属性
AMQP.BasicProperties properties = new AMQP.BasicProperties.Builder()
        .headers(headers)
        .build();

// 发送消息到交换机，不需要指定路由键
channel.basicPublish(EXCHANGE_NAME, "", properties, message.getBytes());
```

**消费者**

```java
// 创建通道
Channel channel = connection.createChannel()) {

// 声明头部交换机，类型为Headers
channel.exchangeDeclare(EXCHANGE_NAME, "headers");

// 声明队列
channel.queueDeclare(QUEUE_NAME, false, false, false, null);

// 设置队列的匹配条件，指定头部属性
Map<String, Object> headers = new HashMap<>();
headers.put("header1", "value1");
headers.put("header2", "value2");

// 将队列绑定到交换机，并指定匹配条件为指定的头部属性
channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, "", headers);

// 创建消费者
DeliverCallback deliverCallback = (consumerTag, delivery) -> {
    String message = new String(delivery.getBody(), "UTF-8");
    System.out.println(" [x] Received '" + message + "' with headers: " + delivery.getProperties().getHeaders());
};

// 消费消息
channel.basicConsume(QUEUE_NAME, true, deliverCallback, consumerTag -> {
});
```


