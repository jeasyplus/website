# Java多线程

## Java线程的四种实现方式

**继承线程类**
```java
class MyThread extends Thread {
    public void run() {
        // 线程的执行逻辑
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread thread = new MyThread();
        thread.start();
    }
}
```
对线程的生命周期有较高的控制要求。

**实现线程接口**
```java
class MyRunnable implements Runnable {
    public void run() {
        // 线程的执行逻辑
    }
}

public class Main {
    public static void main(String[] args) {
        MyRunnable runnable = new MyRunnable();
        Thread thread = new Thread(runnable);
        thread.start();
    }
}
```
多个线程可共享同一个Runnable实例，同时，避免了Java单继承的限制。

**使用Callable和Future**
Callable接口定义线程的执行逻辑，Future对象获取执行结果。
```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

class MyCallable implements Callable<String> {
    public String call() {
        // 线程的执行逻辑
        return "Result";
    }
}

public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newSingleThreadExecutor();
        Future<String> future = executor.submit(new MyCallable());
        // 获取执行结果
        try {
            String result = future.get();
            System.out.println(result);
        } catch (Exception e) {
            e.printStackTrace();
        }
        executor.shutdown();
    }
}
```
适用于需要线程返回结果并进行后续处理的情况。

**使用线程池**

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(5);
        for (int i = 0; i < 10; i++) {
            executor.execute(new Runnable() {
                public void run() {
                    // 线程的执行逻辑
                }
            });
        }
        executor.shutdown();
    }
}
```
适用于需要频繁执行多个任务的场景。