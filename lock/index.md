# Java同步原语

多线程编程中实现线程同步和互斥和基本工具是同步原语，通常有以下几种同步原语：

## 互斥锁（Mutex）：

互斥锁是一种最基本的同步原语，用于保护共享资源，确保一次只有一个线程可以访问临界区代码。当一个线程获得互斥锁后，其他线程必须等待锁的释放才能继续执行。

在Java中，互斥锁可以使用内置的synchronized关键字或java.util.concurrent.locks包中的ReentrantLock类来实现。

**synchronized关键字实现互斥锁：**
```java
public class MutexExample {
    private static final Object mutex = new Object();

    public void criticalSection() {
        synchronized (mutex) {
            // 在这里编写需要互斥访问的代码
            // 只有一个线程可以同时执行这段代码
        }
    }
}
```

**ReentrantLock实现互斥锁：**
```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class MutexExample {
    private final Lock lock = new ReentrantLock();

    public void criticalSection() {
        lock.lock();
        try {
            // 在这里编写需要互斥访问的代码
            // 只有一个线程可以同时执行这段代码
        } finally {
            lock.unlock();
        }
    }
}
```
这样只有一个线程可以获取锁并执行临界区代码。

无论使用**synchronized**关键字还是**ReentrantLock**类，都可以实现互斥锁的效果，确保在同一时间只有一个线程可以执行临界区代码，以避免数据竞争和并发问题。

## 信号量（Semaphore）

信号量是一种通用的同步原语，它用于控制对多个资源的访问。信号量维护一个计数器，可以限制并发访问资源的线程数量，并提供 P（acquire）和 V（release）操作来获取和释放资源。

在Java中，可以使用java.util.concurrent.Semaphore类来实现信号量。下面是一个使用信号量的简单示例：

**Semaphore类来实现信号量**
```java
import java.util.concurrent.Semaphore;

public class SemaphoreExample {
    private static final int NUM_THREADS = 5;
    private static final int MAX_PERMITS = 2;
    private static final Semaphore semaphore = new Semaphore(MAX_PERMITS);

    public static void main(String[] args) {
        for (int i = 0; i < NUM_THREADS; i++) {
            Thread thread = new Thread(new WorkerThread(i));
            thread.start();
        }
    }

    static class WorkerThread implements Runnable {
        private int id;

        public WorkerThread(int id) {
            this.id = id;
        }

        @Override
        public void run() {
            try {
                System.out.println("Thread " + id + " is waiting to acquire permit.");
                semaphore.acquire();
                System.out.println("Thread " + id + " has acquired permit.");

                // 执行需要许可的操作
                Thread.sleep(2000);

                System.out.println("Thread " + id + " is releasing permit.");
                semaphore.release();
                System.out.println("Thread " + id + " has released permit.");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```
在上述示例中，我们创建了一个Semaphore对象，并初始化了最大许可数为2。然后，我们创建了5个工作线程，并在每个线程中执行如下操作：

+ **线程尝试获取许可：** 通过调用acquire()方法获取信号量的许可。如果当前许可数大于0，线程可以继续执行；否则，线程将被阻塞，直到有许可可用。
+ **执行需要许可的操作：** 这里我们使用Thread.sleep()模拟需要许可才能执行的操作。
+ **线程释放许可：** 通过调用release()方法释放信号量的许可。这样其他线程就可以获取许可并执行操作。
通过信号量，我们可以限制并发访问的线程数量，这里最多允许两个线程同时执行需要许可的操作。其他线程必须等待许可的释放才能继续执行。这种方式可以用于资源的限制、任务调度等场景，以实现线程间的协作和控制。

## 条件变量（Condition Variable）

条件变量用于在线程之间进行等待和通知。它通常与互斥锁一起使用，允许线程等待某个条件的发生，并在条件满足时被其他线程通知唤醒。

在Java中，条件变量可以使用java.util.concurrent.locks.Condition接口及其实现类来实现。下面是一个简单的示例：

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ConditionVariableExample {
    private final Lock lock = new ReentrantLock();
    private final Condition condition = lock.newCondition();
    private boolean flag = false;

    public void waitForFlag() throws InterruptedException {
        lock.lock();
        try {
            while (!flag) {
                condition.await(); // 等待条件满足
            }
        } finally {
            lock.unlock();
        }
        System.out.println("Flag is true. Continue execution.");
    }

    public void setFlag() {
        lock.lock();
        try {
            flag = true;
            condition.signalAll(); // 通知等待的线程条件已满足
        } finally {
            lock.unlock();
        }
    }

    public static void main(String[] args) throws InterruptedException {
        ConditionVariableExample example = new ConditionVariableExample();

        Thread thread1 = new Thread(() -> {
            try {
                example.waitForFlag();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        Thread thread2 = new Thread(() -> {
            example.setFlag();
        });

        thread1.start(); // 启动线程1，等待条件满足
        Thread.sleep(1000); // 确保线程1先运行
        thread2.start(); // 启动线程2，设置条件满足
    }
}
```

条件变量（Condition Variable）是一种同步原语，用于在线程之间进行等待和通知。它通常与互斥锁（Mutex）结合使用，用于实现更复杂的线程间协作。

在Java中，条件变量可以使用java.util.concurrent.locks.Condition接口及其实现类来实现。下面是一个简单的示例：

java
Copy code
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ConditionVariableExample {
private final Lock lock = new ReentrantLock();
private final Condition condition = lock.newCondition();
private boolean flag = false;

    public void waitForFlag() throws InterruptedException {
        lock.lock();
        try {
            while (!flag) {
                condition.await(); // 等待条件满足
            }
        } finally {
            lock.unlock();
        }
        System.out.println("Flag is true. Continue execution.");
    }

    public void setFlag() {
        lock.lock();
        try {
            flag = true;
            condition.signalAll(); // 通知等待的线程条件已满足
        } finally {
            lock.unlock();
        }
    }

    public static void main(String[] args) throws InterruptedException {
        ConditionVariableExample example = new ConditionVariableExample();

        Thread thread1 = new Thread(() -> {
            try {
                example.waitForFlag();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        Thread thread2 = new Thread(() -> {
            example.setFlag();
        });

        thread1.start(); // 启动线程1，等待条件满足
        Thread.sleep(1000); // 确保线程1先运行
        thread2.start(); // 启动线程2，设置条件满足
    }
}
在上述示例中，我们创建了一个ConditionVariableExample对象，其中包含一个互斥锁（lock）和一个条件变量（condition）。flag变量表示条件是否满足。

在waitForFlag()方法中，线程获取互斥锁后，如果条件不满足，则调用await()方法等待条件满足。一旦条件满足，线程将被唤醒并继续执行后续代码。

在setFlag()方法中，线程获取互斥锁后，将flag设置为true，然后通过signalAll()方法通知等待的线程条件已满足。

在main()方法中，我们创建了两个线程：thread1和thread2。thread1等待条件满足，而thread2设置条件满足。通过互斥锁和条件变量的配合，thread1将在条件满足后继续执行。

条件变量提供了一种有效的机制，使线程能够等待特定条件的发生，并在条件满足时被唤醒。它在需要线程间协作和同步的情况下非常有用，例如生产者-消费者问题、任务调度等。

## 屏障（Barrier）

屏障用于协调多个线程的执行，确保在所有线程到达屏障点之前，它们都会被阻塞。一旦所有线程都到达屏障点，它们将一起继续执行。

```java
CyclicBarrier barrier = new CyclicBarrier(NUM_THREADS, barrierAction);
```

在多线程编程中，屏障可以用于实现以下场景：

+ 同步点：当需要确保所有线程都执行到某个特定位置时，可以使用屏障。例如，在多个线程计算部分结果后，需要等待所有线程都完成计算后再进行下一步的合并操作。

+ 数据分析和处理：当需要将数据分割成多个任务并并行处理时，屏障可以用于确保所有任务都完成后再进行结果的合并和汇总。

+ 游戏编程：屏障可以用于控制多个玩家线程在游戏中达到某个关键事件或位置后再同时继续执行，以保持游戏的同步性。

在Java中，可以使用java.util.concurrent.CyclicBarrier类来实现屏障。下面是一个简单的示例：

```java
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class BarrierExample {
    private static final int NUM_THREADS = 3;

    public static void main(String[] args) {
        Runnable barrierAction = () -> System.out.println("All threads have reached the barrier.");

        CyclicBarrier barrier = new CyclicBarrier(NUM_THREADS, barrierAction);

        for (int i = 0; i < NUM_THREADS; i++) {
            Thread thread = new Thread(new WorkerThread(barrier));
            thread.start();
        }
    }

    static class WorkerThread implements Runnable {
        private CyclicBarrier barrier;

        public WorkerThread(CyclicBarrier barrier) {
            this.barrier = barrier;
        }

        @Override
        public void run() {
            try {
                System.out.println("Thread has started.");
                // 模拟工作
                Thread.sleep(2000);
                System.out.println("Thread has reached the barrier.");
                barrier.await(); // 等待其他线程到达屏障点
                System.out.println("Thread has resumed execution.");
            } catch (InterruptedException | BrokenBarrierException e) {
                e.printStackTrace();
            }
        }
    }
}
```
在上述示例中，我们创建了一个CyclicBarrier对象，并指定了屏障点的数量为NUM_THREADS。每个工作线程在执行一段模拟工作后，通过调用await()方法等待其他线程到达屏障点。当所有线程都到达屏障点后，它们将一起继续执行，并执行barrierAction中定义的动作。

屏障可用于协调多个线程的执行，以便在关键点上实现同步和等待。它在需要线程之间的同步和协作，以及在某个条件满足后再同时继续执行的情况下非常有用。

**倒计时锁（CountDownLatch）--同步辅助类**


CountDownLatch是Java中提供的同步辅助类，用于控制线程的等待，直到一组操作完成。

CountDownLatch内部维护一个计数器，该计数器初始化为一个正整数，表示需要等待的操作数。每个线程在完成任务后都会将计数器减一。当计数器值达到零时，等待的线程将被唤醒继续执行。

以下是CountDownLatch的基本用法：

+ 创建一个CountDownLatch对象并指定初始计数器的值。
+ 在需要等待的线程中调用await()方法，该方法将阻塞线程直到计数器值达到零。
+ 在完成操作的线程中调用countDown()方法，该方法将减少计数器的值。

```java
import java.util.concurrent.CountDownLatch;

public class CountDownLatchExample {
    private static final int NUM_THREADS = 3;

    public static void main(String[] args) throws InterruptedException {
        CountDownLatch latch = new CountDownLatch(NUM_THREADS);

        for (int i = 0; i < NUM_THREADS; i++) {
            Thread thread = new Thread(new WorkerThread(latch));
            thread.start();
        }

        // 等待所有线程完成任务
        latch.await();

        System.out.println("All threads have completed their tasks. Continuing execution.");
    }

    static class WorkerThread implements Runnable {
        private CountDownLatch latch;

        public WorkerThread(CountDownLatch latch) {
            this.latch = latch;
        }

        @Override
        public void run() {
            try {
                // 模拟任务执行
                Thread.sleep(2000);
                System.out.println("Thread has completed its task.");

                // 任务完成后减少计数器的值
                latch.countDown();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

在上述示例中，我们创建了一个CountDownLatch对象，并指定初始计数器值为NUM_THREADS（这里是3）。每个工作线程在完成任务后，调用countDown()方法减少计数器的值。

在main()方法中，我们等待所有线程完成任务，通过调用latch.await()方法阻塞主线程，直到计数器值达到零。

一旦所有线程完成任务并调用了countDown()方法减少计数器的值，主线程将被唤醒继续执行，并打印出"All threads have completed their tasks. Continuing execution."的消息。

这个示例演示了使用CountDownLatch来等待一组线程完成任务后再继续执行的基本用法。


## 读写锁（Read-Write Lock）

读写锁允许多个线程同时读取共享资源，但只允许一个线程进行写操作。这可以提高读多写少场景下的并发性能。

读写锁（Read-Write Lock）是一种同步原语，用于在读多写少的场景中实现更高的并发性。它允许多个线程同时读取共享资源，但只允许一个线程进行写操作。

**读写锁的主要特点是：**

+ 多个线程可以同时持有读锁，以进行并发的读操作。
+ 写锁是互斥的，只允许一个线程持有写锁，以进行写操作。当有线程持有写锁时，其他线程无法持有读锁或写锁。

这种读多写少的策略可以提高并发性能，因为多个线程可以同时读取资源而无需互斥。但是，当有线程持有写锁时，其他线程无法读取或写入，以确保数据一致性和线程安全。

在Java中，可以使用java.util.concurrent.locks.ReentrantReadWriteLock类来实现读写锁。下面是一个简单的示例：

```java
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class ReadWriteLockExample {
    private static final ReadWriteLock lock = new ReentrantReadWriteLock();
    private static int sharedData = 0;

    public static void main(String[] args) {
        Runnable reader = () -> {
            lock.readLock().lock();
            try {
                // 读取共享数据
                System.out.println("Reader thread read: " + sharedData);
            } finally {
                lock.readLock().unlock();
            }
        };

        Runnable writer = () -> {
            lock.writeLock().lock();
            try {
                // 写入共享数据
                sharedData++;
                System.out.println("Writer thread wrote: " + sharedData);
            } finally {
                lock.writeLock().unlock();
            }
        };

        // 创建多个读线程和写线程
        for (int i = 0; i < 3; i++) {
            new Thread(reader).start();
        }
        for (int i = 0; i < 2; i++) {
            new Thread(writer).start();
        }
    }
}
```
在上述示例中，我们创建了一个ReentrantReadWriteLock对象作为读写锁，用于保护共享数据sharedData的访问。

读操作使用读锁（readLock()）进行保护，写操作使用写锁（writeLock()）进行保护。

在多个读线程中，通过调用readLock().lock()获取读锁，然后读取共享数据，最后通过readLock().unlock()释放读锁。

在写线程中，通过调用writeLock().lock()获取写锁，然后写入共享数据，最后通过writeLock().unlock()释放写锁。

这个示例展示了读写锁的基本用法，其中多个线程可以同时读取共享数据，但只允许一个线程进行写操作。读写锁提供了更灵活的并发控制，适用于读多写少的场景，以提高性能和并发性。


