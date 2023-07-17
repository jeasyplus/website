Kafka

## 高水位

高水位（High Watermark）代表了 Kafka 分区中消费者可安全消费的最高消息偏移量。

当生产者将消息发送到 Kafka 分区时，消息的偏移量会递增，并且只有在消息被所有副本成功复制并写入后，高水位才会更新到对应的偏移量。

一旦消费者处理完当前的消息，可以更新其消费偏移量并继续消费下一条消息，直到达到高水位为止。

## kafka选举

**初始选举（Initial Election）：**

在启动 Kafka 集群时，会进行一次初始选举，选举出第一个 Controller。

在初始选举中，每个 Kafka Broker 都可以成为 Controller 候选者，最终根据一定的算法选举出一个 Broker 成为 Controller。

**故障转移选举（Failover Election）：**

在现任的 Controller 失效或不可用时，需要进行故障转移选举，选举出新的 Controller。

故障转移选举涉及到参与选举的 Broker 发送选举请求并进行投票，最终根据一定的选举算法选出新的 Controller。

## kafka角色

**Broker：** Kafka 集群中的每个节点都被称为 Broker。每个 Broker 负责存储消息的分区副本，并处理来自生产者和消费者的消息流。Broker 之间通过 ZooKeeper 进行协调和管理。

**Producer：** 生产者是将消息发送到 Kafka 集群的应用程序。它负责将消息发布到一个或多个主题的分区，并将消息发送到对应分区的 Leader Broker。生产者可以指定消息的键（Key），用于决定消息被分配到哪个分区。

**Consumer：** 消费者是从 Kafka 集群中读取消息的应用程序。消费者可以订阅一个或多个主题，并从分区的 Leader Broker 中消费消息。消费者可以以不同的方式进行消息消费，如批量消费、按时间消费、按偏移量消费等。

**Topic：** 主题是消息的逻辑分类，它是 Kafka 中消息发布和订阅的基本单位。主题由一个或多个分区组成，每个分区在多个 Broker 上都有副本。生产者将消息发布到特定主题，消费者则通过订阅主题来接收消息。

**Partition：** 分区是主题的物理部分，每个主题可以被划分为一个或多个分区。每个分区在一个 Broker 上有一个 Leader 副本，以及零个或多个 Follower 副本。分区是 Kafka 实现高吞吐量和伸缩性的关键机制。

**Leader 和 Follower：** 每个分区都有一个 Leader 副本和零个或多个 Follower 副本。Leader 副本负责处理读写请求，而 Follower 副本负责复制 Leader 的数据。当 Leader 失效时，Follower 中的一个会被选举为新的 Leader。

**Controller：** Controller 是 Kafka 集群中的一个特殊角色，负责管理整个集群的元数据信息、分区分配和副本管理等任务。Controller 的选举和故障转移是通过 ZooKeeper 来实现的。

## kafka ISR 

“In-Sync Replica”（同步副本）的简称。

ISR 的概念主要用于数据的可靠性和一致性。

通过 ISR 机制，Kafka 能够提供数据的持久性和一致性保证，同时在副本之间实现高效的数据复制和同步。

ISR 是指当前与 Leader 副本保持同步的副本集合，它包括了那些已经追上 Leader 副本并且与 Leader 副本保持一定程度的同步的副本。

每个 Kafka 分区有一个 Leader 副本和零个或多个 Follower 副本。

当生产者发送消息到分区时，它将被写入 Leader 副本，并且 Leader 副本负责处理消费者的读写请求。为了保证数据的可靠性和容错性，Kafka 使用副本机制将消息复制到其他副本。

ISR 是 Leader 副本的一个子集，它包含了那些已经追上 Leader 副本并且与 Leader 副本保持一定程度同步的副本。

只有 ISR 中的副本才能成为 Leader 候选者，也就是只有 ISR 中的副本才有资格成为下一次 Leader。

当某个副本与 Leader 副本之间的同步延迟超过一定阈值或副本发生故障时，该副本将被从 ISR 中移除。只有 ISR 中的副本才能保证在发生故障或 Leader 副本切换时能够保持数据的一致性，因为它们与 Leader 副本保持同步。

