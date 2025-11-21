# Java多线程编程

本教程将学习Java中的多线程编程，包括线程创建、同步、通信、线程池等。

## 1. 线程创建

### 继承Thread类

```java
class MyThread extends Thread {
    private String name;
    
    public MyThread(String name) {
        this.name = name;
    }
    
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(name + ": " + i);
            try {
                Thread.sleep(1000);  // 休眠1秒
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class ThreadDemo {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread("线程1");
        MyThread thread2 = new MyThread("线程2");
        
        thread1.start();  // 启动线程
        thread2.start();  // 启动线程
        
        // 等待线程完成
        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        System.out.println("所有线程完成");
    }
}
```

### 实现Runnable接口

```java
class MyRunnable implements Runnable {
    private String name;
    
    public MyRunnable(String name) {
        this.name = name;
    }
    
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(name + ": " + i);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class RunnableDemo {
    public static void main(String[] args) {
        Thread thread1 = new Thread(new MyRunnable("线程1"));
        Thread thread2 = new Thread(new MyRunnable("线程2"));
        
        thread1.start();
        thread2.start();
        
        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

### 使用Lambda表达式（Java 8+）

```java
public class LambdaThread {
    public static void main(String[] args) {
        // 使用Lambda表达式创建线程
        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Lambda线程: " + i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        
        thread1.start();
        
        try {
            thread1.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

## 2. 线程生命周期

```java
public class ThreadLifecycle {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        
        // NEW：新建状态
        System.out.println("状态: " + thread.getState());  // NEW
        
        thread.start();
        
        // RUNNABLE：可运行状态
        System.out.println("状态: " + thread.getState());  // RUNNABLE
        
        // 等待线程完成
        try {
            thread.join();
            // TERMINATED：终止状态
            System.out.println("状态: " + thread.getState());  // TERMINATED
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

## 3. 线程同步

### synchronized关键字

```java
class Counter {
    private int count = 0;
    
    // synchronized方法：同一时间只有一个线程可以执行
    public synchronized void increment() {
        count++;
    }
    
    // synchronized代码块
    public void increment2() {
        synchronized (this) {
            count++;
        }
    }
    
    public int getCount() {
        return count;
    }
}

public class SynchronizedDemo {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();
        
        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });
        
        Thread thread2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });
        
        thread1.start();
        thread2.start();
        
        thread1.join();
        thread2.join();
        
        System.out.println("最终计数: " + counter.getCount());  // 2000
    }
}
```

### Lock接口（Java 5+）

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class CounterWithLock {
    private int count = 0;
    private Lock lock = new ReentrantLock();
    
    public void increment() {
        lock.lock();  // 加锁
        try {
            count++;
        } finally {
            lock.unlock();  // 解锁（确保在finally中解锁）
        }
    }
    
    public int getCount() {
        return count;
    }
}

public class LockDemo {
    public static void main(String[] args) throws InterruptedException {
        CounterWithLock counter = new CounterWithLock();
        
        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });
        
        Thread thread2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });
        
        thread1.start();
        thread2.start();
        
        thread1.join();
        thread2.join();
        
        System.out.println("最终计数: " + counter.getCount());
    }
}
```

## 4. 线程通信

### wait和notify

```java
import java.util.Queue;
import java.util.LinkedList;

class ProducerConsumer {
    private Queue<Integer> queue = new LinkedList<>();
    private int capacity = 5;
    
    public synchronized void produce(int item) throws InterruptedException {
        while (queue.size() == capacity) {
            wait();  // 等待空间
        }
        queue.add(item);
        System.out.println("生产: " + item);
        notifyAll();  // 通知消费者
    }
    
    public synchronized int consume() throws InterruptedException {
        while (queue.isEmpty()) {
            wait();  // 等待数据
        }
        int item = queue.poll();
        System.out.println("消费: " + item);
        notifyAll();  // 通知生产者
        return item;
    }
}

public class WaitNotifyDemo {
    public static void main(String[] args) {
        ProducerConsumer pc = new ProducerConsumer();
        
        // 生产者线程
        Thread producer = new Thread(() -> {
            try {
                for (int i = 0; i < 10; i++) {
                    pc.produce(i);
                    Thread.sleep(500);
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        
        // 消费者线程
        Thread consumer = new Thread(() -> {
            try {
                for (int i = 0; i < 10; i++) {
                    pc.consume();
                    Thread.sleep(1000);
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        
        producer.start();
        consumer.start();
        
        try {
            producer.join();
            consumer.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

### 条件变量（Condition）

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.Queue;
import java.util.LinkedList;

class ProducerConsumerWithCondition {
    private Queue<Integer> queue = new LinkedList<>();
    private int capacity = 5;
    private Lock lock = new ReentrantLock();
    private Condition notFull = lock.newCondition();
    private Condition notEmpty = lock.newCondition();
    
    public void produce(int item) throws InterruptedException {
        lock.lock();
        try {
            while (queue.size() == capacity) {
                notFull.await();  // 等待不满
            }
            queue.add(item);
            System.out.println("生产: " + item);
            notEmpty.signal();  // 通知不空
        } finally {
            lock.unlock();
        }
    }
    
    public int consume() throws InterruptedException {
        lock.lock();
        try {
            while (queue.isEmpty()) {
                notEmpty.await();  // 等待不空
            }
            int item = queue.poll();
            System.out.println("消费: " + item);
            notFull.signal();  // 通知不满
            return item;
        } finally {
            lock.unlock();
        }
    }
}

public class ConditionDemo {
    public static void main(String[] args) {
        ProducerConsumerWithCondition pc = new ProducerConsumerWithCondition();
        
        Thread producer = new Thread(() -> {
            try {
                for (int i = 0; i < 10; i++) {
                    pc.produce(i);
                    Thread.sleep(500);
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        
        Thread consumer = new Thread(() -> {
            try {
                for (int i = 0; i < 10; i++) {
                    pc.consume();
                    Thread.sleep(1000);
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        
        producer.start();
        consumer.start();
        
        try {
            producer.join();
            consumer.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

## 5. 线程池

### ExecutorService

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class ThreadPoolDemo {
    public static void main(String[] args) {
        // 创建线程池
        ExecutorService executor = Executors.newFixedThreadPool(5);
        
        // 提交任务
        for (int i = 0; i < 10; i++) {
            final int taskId = i;
            executor.submit(() -> {
                System.out.println("任务 " + taskId + " 在线程 " 
                                 + Thread.currentThread().getName() + " 执行");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            });
        }
        
        // 关闭线程池
        executor.shutdown();
        
        try {
            // 等待所有任务完成
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                executor.shutdownNow();
            }
        } catch (InterruptedException e) {
            executor.shutdownNow();
        }
    }
}
```

### Callable和Future

```java
import java.util.concurrent.*;

public class CallableFutureDemo {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(5);
        
        // Callable：有返回值的任务
        Callable<Integer> task = () -> {
            int sum = 0;
            for (int i = 1; i <= 100; i++) {
                sum += i;
            }
            Thread.sleep(2000);
            return sum;
        };
        
        // 提交任务，返回Future
        Future<Integer> future = executor.submit(task);
        
        System.out.println("任务已提交，等待结果...");
        
        try {
            // 获取结果（会阻塞直到完成）
            Integer result = future.get();
            System.out.println("结果: " + result);
            
            // 带超时的获取
            // Integer result = future.get(5, TimeUnit.SECONDS);
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace();
        }
        
        executor.shutdown();
    }
}
```

## 6. 原子类（Atomic）

```java
import java.util.concurrent.atomic.AtomicInteger;

class AtomicCounter {
    private AtomicInteger count = new AtomicInteger(0);
    
    public void increment() {
        count.incrementAndGet();  // 原子操作
    }
    
    public int getCount() {
        return count.get();
    }
}

public class AtomicDemo {
    public static void main(String[] args) throws InterruptedException {
        AtomicCounter counter = new AtomicCounter();
        
        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });
        
        Thread thread2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });
        
        thread1.start();
        thread2.start();
        
        thread1.join();
        thread2.join();
        
        System.out.println("最终计数: " + counter.getCount());  // 2000
    }
}
```

## 7. 综合示例

### 生产者-消费者完整实现

```java
import java.util.concurrent.*;
import java.util.concurrent.atomic.AtomicInteger;

class Producer implements Runnable {
    private BlockingQueue<Integer> queue;
    private AtomicInteger counter;
    
    public Producer(BlockingQueue<Integer> queue, AtomicInteger counter) {
        this.queue = queue;
        this.counter = counter;
    }
    
    @Override
    public void run() {
        try {
            while (true) {
                int item = counter.incrementAndGet();
                queue.put(item);  // 阻塞直到有空间
                System.out.println("生产者: 生产 " + item);
                Thread.sleep(1000);
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

class Consumer implements Runnable {
    private BlockingQueue<Integer> queue;
    
    public Consumer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }
    
    @Override
    public void run() {
        try {
            while (true) {
                int item = queue.take();  // 阻塞直到有数据
                System.out.println("消费者: 消费 " + item);
                Thread.sleep(1500);
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

public class ProducerConsumerComplete {
    public static void main(String[] args) {
        BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(10);
        AtomicInteger counter = new AtomicInteger(0);
        
        ExecutorService executor = Executors.newFixedThreadPool(4);
        
        // 创建生产者和消费者
        executor.submit(new Producer(queue, counter));
        executor.submit(new Producer(queue, counter));
        executor.submit(new Consumer(queue));
        executor.submit(new Consumer(queue));
        
        // 运行一段时间后关闭
        try {
            Thread.sleep(10000);
            executor.shutdownNow();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

## 练习

1. 实现一个线程安全的消息队列
2. 使用多线程计算数组的和
3. 实现一个线程池
4. 使用Callable和Future实现并行计算
5. 实现生产者-消费者模式的完整版本

