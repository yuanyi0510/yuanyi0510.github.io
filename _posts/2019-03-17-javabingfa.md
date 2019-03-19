---
layout: post
title: "Java并发编程的艺术"
date: 2019-03-17   
tag: "摘要" 
---

[TOC]



### Java中的并发工具类

#### 等待多线程完成的CountDownLatch

CountDownLatch允许一个或多个线程等待其他线程完成操作，可以实现一个计数器的功能，但要注意，计数器必须大于等于0，等于0的时候，调用await方法不会阻塞。CountDownLatch不可能重新初始化或者修改CountDownLatch对象的内部计数器的值，一个线程调用countDown方法happen-before另一个线程调用await方法

看一下源码：

```java
public class CountDownLatch {
    /**
     * 首先CountDownLatch和其他锁机制一样实现了队列同步器AQS，通过他的setState、getCount、和tryxxx方法实现同步操作
     */
    private static final class Sync extends AbstractQueuedSynchronizer {
        private static final long serialVersionUID = 4982264981922014374L;

        Sync(int count) {
            setState(count);
        }

        int getCount() {
            return getState();
        }
//共享式的获取同步状态，如果count为0就获取锁
        protected int tryAcquireShared(int acquires) {
            return (getState() == 0) ? 1 : -1;
        }
//共享式的释放锁
        protected boolean tryReleaseShared(int releases) {
            // count减1，直达count为0
            for (;;) {
                int c = getState();
                if (c == 0)
                    return false;
                int nextc = c-1;
              //compareAndSetState保证了原子操作
                if (compareAndSetState(c, nextc))
                    return nextc == 0;
            }
        }
    }

    private final Sync sync;

    /**
     *构造器需要接收一个int类型的参数作为计数器，不能为负数
     */
    public CountDownLatch(int count) {
        if (count < 0) throw new IllegalArgumentException("count < 0");
        this.sync = new Sync(count);
    }

    /**
     * 使当前线程阻塞，直到count为0才继续执行
     */
    public void await() throws InterruptedException {
        sync.acquireSharedInterruptibly(1);
    }

    /**
     * 在指定的时间内count还没为0，当前线程继续执行
     */
    public boolean await(long timeout, TimeUnit unit)
        throws InterruptedException {
        return sync.tryAcquireSharedNanos(1, unit.toNanos(timeout));
    }

    /**
     * 使count-1
     */
    public void countDown() {
        sync.releaseShared(1);
    }

    public long getCount() {
        return sync.getCount();
    }

    public String toString() {
        return super.toString() + "[Count = " + sync.getCount() + "]";
    }
}

```

  #### 同步屏障CyclicBarrier

让一组线程到达一个屏障时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被拦截的线程才会继续执行。可以用于多线程计算数据最后合并计算结果的场景。

源码分析（列举一些常见的方法）：

```java
 public class CyclicBarrier {
   //内部使用重入锁
   ……
   //parties指屏障的线程数，barrierAction在线程到达屏障时，优先执行
    public CyclicBarrier(int parties, Runnable barrierAction) {
        if (parties <= 0) throw new IllegalArgumentException();
        this.parties = parties;
        this.count = parties;
        this.barrierCommand = barrierAction;
    }
   //一个参数的构造方法
    public CyclicBarrier(int parties) {
        this(parties, null);
    }
   //用来挂起当前线程，直至所有线程都到达barrier状态再同时执行后续任务；也有传入限制时间的await
   public int await() throws InterruptedException, BrokenBarrierException {
        try {
            return dowait(false, 0L);
        } catch (TimeoutException toe) {
            throw new Error(toe); // cannot happen
        }
    }
   //用于重置计数器，如果有正在被屏障的线程，则会抛出BrokenBarrierException异常
   public void reset() {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            breakBarrier();   // break the current generation
            nextGeneration(); // start a new generation
        } finally {
            lock.unlock();
        }
    }
   // 查看被屏障的线程是否被中断
   public boolean isBroken() {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            return generation.broken;
        } finally {
            lock.unlock();
        }
    }
 }
```

#### 控制并发线程数的Semaphore

信号量，用来控制同时访问特定资源的线程数量。可以用于流量控制，比如数据库连接。Semaphore（int permits）构造方法中的参数标识可用的许可证数量也就是最大并发数

### Java中的线程池

#### 线程池的实现原理

![线程池的处理流程](https://raw.githubusercontent.com/yuanyi0510/yuanyi0510.github.io/master/images/bolg_images/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/java%E5%B9%B6%E5%8F%91%E7%9A%84%E8%89%BA%E6%9C%AF/1.jpg)

#### 线程池的使用

##### 创建

通过ThreadPoolExecutor创建，参数如下：

1. corePoolSIze：线程池的基本大小
2. runnableTaskQueue：任务队列，用于报讯等待执行任务的阻塞队列
   1. ArrayBlockingQueue：基于数组的有界阻塞队列
   2. LinkedBlockingQueue：基于链表的阻塞队列，吞吐量较高。固定线程池使用了这个队列
   3. SynchronousQueue：不存储元素的阻塞队列。可缓存的线程池使用的这个队列，每次插入必须等到另一个线程调用移除，否则一直阻塞
   4. PriorityBlockingQueue：具有优先级的无限阻塞队列
3. maximumPoolSize：线程池最大数量
4. RejectedExecutionHandler：饱和策略。
   1. AbortPolicy：直接抛出异常
   2. CallerRunsPolicy：只用调用者所在的线程执行任务
   3. DiscardOldestPolicy：丢弃队列中最近的一个任务，并执行当前任务
   4. DiscardPolicy：不处理，丢弃
5. keepAliveTime：存活时间，工作线程空闲之后保持存活的时间
6. TimeUnit：保持时间的单位

##### 提交任务

- execute（）：没有返回值，无法确认任务是否执行成功
- submit（）：会返回一个future对象，通过get方法获取返回值

##### 关闭

- shutdown：将线程池的状态设置为SHUTDOWN状态，中断所有没有正在执行任务的线程
- shutdownNow：停止所有正在执行任务或者暂停任务的线程，并反hi等待执行任务的列表

### Executor框架

两级调度框架：

![两级调度框架](https://raw.githubusercontent.com/yuanyi0510/yuanyi0510.github.io/master/images/bolg_images/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/java%E5%B9%B6%E5%8F%91%E7%9A%84%E8%89%BA%E6%9C%AF/2.jpg)

上层：Java程序将应用分解为多个任务

下层：操作系统将线程映射到处理器上

框架的类和接口：

![类和接口](https://raw.githubusercontent.com/yuanyi0510/yuanyi0510.github.io/master/images/bolg_images/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/java%E5%B9%B6%E5%8F%91%E7%9A%84%E8%89%BA%E6%9C%AF/3.jpg)


#### 线程池种类
- FixedThreadPool 
- SingleThreadPool
- CacheedThreadPool


#### ScheduledThreadPoolExecutor 定期执行任务，或者给定延迟之后执行任务

- ScheduledThreadPoolExecutor  适用于多个后台执行周期任务
- SingleThreadScheduledExecutor 单个后台线程执行周期任务，同时保证顺序执行各个任务

![4](https://raw.githubusercontent.com/yuanyi0510/yuanyi0510.github.io/master/images/bolg_images/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/java%E5%B9%B6%E5%8F%91%E7%9A%84%E8%89%BA%E6%9C%AF/4.jpg)






