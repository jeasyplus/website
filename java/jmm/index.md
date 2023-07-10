# Java内存模型（Java Memory Model，简称JMM）

定义了Java程序中多线程**并发访问**共享**变量**时的行为规范。它确保了在不同线程之间的**可见性**、**有序性**和**原子性**。

## Java内存模型的几个关键特性：

**主内存（Main Memory）：** 主内存是所有线程共享的内存区域，用于存储共享的变量。

**线程工作内存（Thread's Working Memory）：** 每个线程都有自己的工作内存，用于存储变量的本地副本和缓存。线程对变量的操作都在工作内存中进行。

**可见性（Visibility）：** JMM保证对于volatile变量的写操作对于其他线程是可见的。当一个线程写入一个volatile变量时，它会强制刷新本地工作内存，并将变量的值写回主内存，使得其他线程能够看到最新的值。

**有序性（Ordering）：** JMM保证volatile变量的读写操作具有顺序性，即在多线程环境下，对于单个volatile变量的读写操作具有全局有序性。

**原子性（Atomicity）：** JMM保证volatile变量的读写操作是原子的。对于volatile变量的读操作和写操作都不会被中断，可以看作是单个不可分割的操作。

**Happens-Before关系：** Happens-Before关系定义了多线程操作之间的顺序规则。如果一个操作的结果对于另一个操作是可见的，并且这两个操作之间存在Happens-Before关系，那么第一个操作的执行顺序必须在第二个操作之前。

Java内存模型定义了一组规则和保证，以确保多线程环境下的可见性、有序性和原子性。开发者可以依赖这些规则来编写正确且线程安全的多线程代码。同时，JMM也提供了一些同步原语，如synchronized关键字、锁、原子类等，来帮助控制线程之间的访问和协调，以实现线程安全和正确的并发操作。

## Happens-Before关系

Happens-Before关系是Java内存模型中定义的一种偏序关系，用于规定多线程程序中操作之间的执行顺序。如果一个操作A Happens-Before另一个操作B，那么我们可以确保在程序执行过程中，操作A的执行结果对于操作B是可见的。

Happens-Before关系具有以下几个规则：

+ 程序顺序规则（Program Order Rule）：在同一个线程中，按照程序顺序，前面的操作Happens-Before后面的操作。

+ 监视器锁规则（Monitor Lock Rule）：对于一个锁的解锁操作Happens-Before后续对于同一个锁的加锁操作。也就是说，对于共享的锁，一个线程解锁操作的结果对于随后的锁的加锁操作是可见的。

+ volatile变量规则（Volatile Variable Rule）：对于一个volatile变量的写操作Happens-Before后续对于同一个变量的读操作。也就是说，对于volatile变量的写操作的结果对于随后的读操作是可见的。

+ 线程启动规则（Thread Start Rule）：一个线程的启动操作Happens-Before于该线程的任何操作。

+ 线程终止规则（Thread Termination Rule）：一个线程的所有操作Happens-Before于其他线程检测到该线程的终止。

+ 中断规则（Interruption Rule）：对于线程的中断操作Happens-Before于被中断线程的代码检测到中断事件的发生。

+ 传递性规则（Transitivity）：如果操作A Happens-Before操作B，且操作B Happens-Before操作C，那么可以推断出操作A Happens-Before操作C。

Happens-Before关系提供了一种有序性的保证，以帮助开发者编写正确的多线程程序。通过遵守Happens-Before关系的规则，可以确保线程间的操作顺序正确，并且对于操作之间的可见性有明确定义。这有助于避免一些常见的多线程并发问题，如数据竞争和内存一致性错误。