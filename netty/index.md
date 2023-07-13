# Netty

Netty基于事件驱动的异步网络应用框架，能快速开发高性能、高可靠性的网络程序。

一般用作RPC框架、实现即时通讯以及实时消息推送等。

相比Java NIO，Netty更方便、性能更高，能承载高并发。

## Netty性能出色的原因

+ 采用IO多路复用技术，将阻塞IO复用到select线程上，有效应对大量的并发请求,
+ 支持多种Reactor线程模型，根据业务场景自行选择
+ 减少不必要的内存拷贝
+ 使用直接内存，且可重复利用
+ 无锁串行化设计，避免锁带来开销
+ 支持 protobuf 等高性能序列化协议

## Netty零拷贝如何实现

+ 直接使用堆外内存，避免 JVM 堆内到堆外的数据拷贝。
+ 组合多个 Buffer 对象合并成一个逻辑对象，避免通过拷贝将几个 Buffer 合并成一个大的 Buffer。
+ 可以将 byte 数组包装成 ByteBuf 对象，包装过程中不产生内存拷贝。
+ 切分过程中不会产生内存拷贝，底层共享一个 byte 数组的存储空间。
+ 操作系统级别的零拷贝，使用 FileChannel#transferTo() 方法，将文件缓冲区的数据直接传输到目标 Channel，避免内核缓冲区到用户态缓冲区之间的数据拷贝。


## Netty的无锁化设计

+ Netty基于Reactor线程模式，实现并发请求处理，避免了线程阻塞与锁的竞争。

+ 使用对象池，减少对象的创建和销毁，同时避免了锁的竞争。

+ 使用CAS和原子类来代替锁，实现线程安全。

+ 许多组件都被设计为线程安全的，例如，每个Channel都有一个唯一的EventLoop，有效避免锁竞争和线程切换带来的开销。

## Netty的线程模型

Netty 使用基于多路复用的 Reactor 模型，接收处理用户的请求。

多路复用即先用阻塞的模式调用系统，询问内核数据是否准备好，如果准备好，再次进行系统调用，对数据进行拷贝。
常见的实现方式有三种：select，epoll和poll。

Netty 的线程模型并不是一成不变，通过设置不同的启动参数，选择不同的模式：
Reactor单线程模型、Reactor多线程模型、Reactor主从多线程模型。

### Reactor 单线程模型：

+ 单线程模型使用一个线程来处理所有的 I/O 事件和业务逻辑。
+ 所有的事件都由一个 Reactor 线程来监听和处理。
+ 适用于连接数较少、业务逻辑简单的场景，对于高并发和复杂的业务逻辑可能存在性能瓶颈。


### Reactor 多线程模型：

+ 多线程模型使用多个线程来处理 I/O 事件和业务逻辑。
+ 通常采用一个 Reactor 线程负责监听事件，将事件分发给多个工作线程进行处理。
+ 每个工作线程都有独立的事件循环，可以并行处理多个事件，提高并发性能。
+ 适用于高并发、需要处理复杂业务逻辑的场景，但需要注意线程安全和数据共享的问题。


### Reactor 主从多线程模型：

+ 主从多线程模型在多线程模型的基础上引入了一个专门的线程池用于处理耗时的业务逻辑。
+ 主线程（Reactor 线程）负责监听和分发事件，将耗时的业务逻辑委托给从线程池中的工作线程处理。
+ 从线程池中的工作线程负责执行具体的业务逻辑，可以充分利用多核 CPU 的计算能力。
+ 适用于高并发、需要处理复杂且耗时的业务逻辑的场景，能够提高并发性能和系统的吞吐量。

总的来说，Reactor 单线程模型适用于简单的应用场景，Reactor 多线程模型适用于处理并发较高、业务逻辑较复杂的场景，而 Reactor 主从多线程模型适用于高并发、耗时的业务逻辑场景。选择适合的模型需要根据实际应用的需求和复杂性进行评估。

### Netty TCP粘包/拆包

Netty通过预先指定的数据流编解码器，按照预先约定好的规则进行数据的解析，即可解决对应的粘包、拆包问题。

具体到代码层面，主要有以下几种解码器：

+ 按照换行符切割报文：LineBasedFrameDecoder
+ 按照自定义分隔符符号切割报文：DelimiterBasedFrameDecoder
+ 按照固定长度切割报文：FixedLenghtFrameDecoder
+ 基于数据包长度切割报文：LengthFieldBasedFrameDecoder

这些解决方案被封装到了handler中，可以基于Netty的责任链模式，进行如下调用即可：

```java
serverBootstrap.group(bossGroup,workerGroup)
.channel(NioServerSocketChannel.class)
.childHandler(new ChannelInitializer<SocketChannel>(){
    @Override
    public void initChannel(SocketChannel channel){
        channel.pipeline.addLast(new FixedLenghtFrameDecoder());
    }
})
```
## Netty序列化协议

+ Java 序列化（Java Serialization）：Netty支持使用 Java 自带的序列化机制。它是一种基于字节流的序列化方式，可以将对象转换为字节流进行传输。

+ JSON（JavaScript Object Notation）：Netty提供了对 JSON 的支持，可以将对象转换为 JSON 格式的字符串，并在网络中进行传输。JSON 是一种轻量级的数据交换格式，易于阅读和解析。

+ Protobuf（Protocol Buffers）：Netty支持使用 Google 的 Protobuf 序列化协议。Protobuf 是一种二进制序列化格式，具有高效、紧凑和跨平台的特性。

+ MessagePack：Netty还支持使用 MessagePack 序列化协议。MessagePack 是一种高效的二进制序列化格式，它比 JSON 更小、更快，适用于数据交换和存储。

+ Kryo：Netty可以与 Kryo 序列化库集成，实现高性能的对象序列化。Kryo 是一个快速、高效的 Java 序列化框架，可以将对象转换为字节流进行传输。








