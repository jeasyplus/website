# ActiveMQ

## Producer（生产者）：

消息的发送者，负责发送消息到 ActiveMQ Broker。

## Broker（消息代理）：

负责接收、存储和分发消息。

消息的中间处理者，将消息从生产者传递给消费者。

一个 ActiveMQ Broker 可以有多个独立的消息队列。

## Queue（消息队列）：

生产者将消息发送到队列，消费者从队列中接收和处理消息。

队列采用先进先出（FIFO）的方式来处理消息。

## Topic（主题）：

一种发布/订阅模式的消息通信方式。

生产者将消息发布到主题，所有订阅了该主题的消费者都会收到消息的副本。

## Consumer（消费者）：

消息的接收者。

负责从 ActiveMQ Broker 接收和处理消息。

## Connector（连接器）：

负责连接生产者和消费者与 ActiveMQ Broker 之间的通信。


## ActiveMQ消息模式

### 点对点模式（Queue）：

消息的发送者（Producer）将消息发送到一个指定的队列（Queue）中。

每个消息只能被发送到一个队列，并且只能被该队列中的一个消费者（Consumer）接收和处理。

如果有多个消费者订阅同一个队列，每个消息也只会被其中一个消费者处理。

点对点模式适用于需要确保消息只被一个消费者处理的场景。

```java

// 创建会话
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

// 创建队列
Queue queue = session.createQueue("test.queue");

// 创建消息生产者
MessageProducer producer = session.createProducer(queue);

// 创建消息
TextMessage message = session.createTextMessage("Hello, ActiveMQ!");

// 发送消息
producer.send(message);

```
```java
// 创建会话
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

// 创建队列
Queue queue = session.createQueue("test.queue");

// 创建消息消费者
MessageConsumer consumer = session.createConsumer(queue);

// 接收消息
Message message = consumer.receive();

if (message instanceof TextMessage) {
    TextMessage textMessage = (TextMessage) message;
    System.out.println("Received message: " + textMessage.getText());
} else {
    System.out.println("Received message of unsupported type.");
}
```
### 订阅模式（Topic）：

消息的发送者（Producer）将消息发布到一个指定的主题（Topic）中。

所有订阅了该主题的消费者（Consumer）都会接收该主题上发布的消息。

每个消息会被复制并发送给所有订阅了该主题的消费者，这样每个消费者都会收到该消息的副本。

订阅模式允许消息的广播和多播，即一条消息可以被多个消费者接收。

订阅模式适用于需要将消息广播给多个消费者的场景。

订阅模式（Topic）不支持 ACK（Acknowledgment）机制的。

```java
// 创建会话
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

// 创建主题
Topic topic = session.createTopic("test.topic");

// 创建消息生产者
MessageProducer producer = session.createProducer(topic);

// 创建消息
TextMessage message = session.createTextMessage("Hello, ActiveMQ Topic!");

// 发布消息
producer.send(message);
```
```java
// 创建会话
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

// 创建主题
Topic topic = session.createTopic("test.topic");

// 创建消息消费者
MessageConsumer consumer = session.createConsumer(topic);

// 设置消息监听器
consumer.setMessageListener(new MessageListener() {
    @Override
    public void onMessage(Message message) {
        if (message instanceof TextMessage) {
            TextMessage textMessage = (TextMessage) message;
            try {
                System.out.println("Received topic message: " + textMessage.getText());
            } catch (JMSException e) {
                e.printStackTrace();
            }
        } else {
            System.out.println("Received message of unsupported type.");
        }
    }
});
```


## 协议支持

支持多种消息协议，包括 OpenWire、STOMP、AMQP、MQTT 等。

## 消息选择器（Message Selector）

```java
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
Topic topic = session.createTopic(topicName);

MessageProducer producer = session.createProducer(topic);

TextMessage message1 = session.createTextMessage("This is an important message.");
message1.setStringProperty("type", "important");
message1.setIntProperty("priority", 10);

TextMessage message2 = session.createTextMessage("This is an ordinary message.");
message2.setStringProperty("type", "ordinary");
message2.setIntProperty("priority", 3);

producer.send(message1);
producer.send(message2);
```

```java
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
Topic topic = session.createTopic(topicName);

// 设置消息选择器，只选择 type=important 且 priority>5 的消息
MessageConsumer consumer = session.createConsumer(topic, "type='important' AND priority>5");

consumer.setMessageListener(new MessageListener() {
    @Override
    public void onMessage(Message message) {
        if (message instanceof TextMessage) {
            try {
                TextMessage textMessage = (TextMessage) message;
                System.out.println("Received message: " + textMessage.getText());
            } catch (JMSException e) {
                e.printStackTrace();
            }
        } else {
            System.out.println("Received message of unsupported type.");
        }
    }
});
```