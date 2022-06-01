_ _ _

layout: post
title: 'ReentranLock部分源码分析'
date: 2022-06-01
author: desert
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: reentrantLock

_ _ _



### 1. 加锁，也就是lock（）方法

```java
 final void lock() {
            if (!initialTryLock())
                acquire(1);
        }
```

initialTryLock非公平锁实现，在首先进行加锁的时候进行一次尝试操作，利用cas操作设置AQS的state，设置成功则修改state的值，将持有锁的线程修改为当前线程，代表加锁成功，不成功则查看持有锁的线程是不是自身，如果是将state加一，代表可重入，不是返回false

```java

final boolean initialTryLock() {
            Thread current = Thread.currentThread();
            if (compareAndSetState(0, 1)) { // first attempt is unguarded
                setExclusiveOwnerThread(current);
                return true;
            } else if (getExclusiveOwnerThread() == current) {
                int c = getState() + 1;
                if (c < 0) // overflow
                    throw new Error("Maximum lock count exceeded");
                setState(c);
                return true;
            } else
                return false;
        }
```

如果不成功，进入acquire方法,acquire方法为AQS的实现，首先会进行tryAcquire，如果成功，则不进行下一步

```java
public final void acquire(int arg) {
        if (!tryAcquire(arg))
            acquire(null, arg, false, false, false, 0L);
    }
```

tryAcquire(AQS没有提供实现，具体实现由使用的类进行实现，接下来我们查看AQS在ReentrantLock中非公平锁的实现)，即如果没有加锁（state==0），并且cas操作修改state成功，将当前线程设置为独占此锁的线程

```java
protected final boolean tryAcquire(int acquires) {
            if (getState() == 0 && compareAndSetState(0, acquires)) {
                setExclusiveOwnerThread(Thread.currentThread());
                return true;
            }
            return false;
        }
```

如果tryAcquire失败，则会进入acquire方法

acquire 方法执行流程为

现在首先检查当前节点是否是头节点
   * 如果是这样，确保头部稳定，否则确保有效的前任
   * 如果是第一个或尚未入队，进行try Acquire获取锁
   * 否则，如果节点尚未创建，则创建它
   * 如果尚未入队，尝试入队一次
   * 否则，如果从park状态唤醒，重试（最多 postSpins 次）
   * 如果未设置 WAITING 状态，请设置并重试
   * 否则park并且清楚 WAITING 状态，并检查取消

```java
final int acquire(Node node, int arg, boolean shared,
                      boolean interruptible, boolean timed, long time) {
        Thread current = Thread.currentThread();
        byte spins = 0, postSpins = 0;   // retries upon unpark of first thread
        boolean interrupted = false, first = false;
        Node pred = null;                // predecessor of node when enqueued

        for (;;) {
            //如果不是第一个节点
            if (!first && (pred = (node == null) ? null : node.prev) != null &&
                !(first = (head == pred))) {
                if (pred.status < 0) {
                    cleanQueue();           // predecessor cancelled
                    continue;
                } else if (pred.prev == null) {
                    Thread.onSpinWait();    // ensure serialization
                    continue;
                }
            }
            /**
            如果是第一个节点或者前驱节点为空，再尝试获取一次锁，shared表示是否为共享锁获取
            1. 获取成功： 如果是第一个节点，就将节点的前驱和后继置为空，返回1 表示获取成功
            2. 获取失败：进行下一步
            */
            if (first || pred == null) {
                boolean acquired;
                try {
                    if (shared)
                        acquired = (tryAcquireShared(arg) >= 0);
                    else
                        acquired = tryAcquire(arg);
                } catch (Throwable ex) {
                    cancelAcquire(node, interrupted, false);
                    throw ex;
                }
                if (acquired) {
                    if (first) {
                        node.prev = null;
                        head = node;
                        pred.next = null;
                        node.waiter = null;
                        if (shared)
                            signalNextIfShared(node);
                        if (interrupted)
                            current.interrupt();
                    }
                    return 1;
                }
            }
            //如果node队列为空，分配队列
            if (node == null) {                 // allocate; retry before enqueue
                if (shared)
                    node = new SharedNode();
                else
                    node = new ExclusiveNode();
            }
            //队列不为空，但是前驱节点为空，
            else if (pred == null) {          // try to enqueue
                node.waiter = current;
                Node t = tail;
                node.setPrevRelaxed(t);         // avoid unnecessary fence
                if (t == null)
                    tryInitializeHead();
                else if (!casTail(t, node))
                    node.setPrevRelaxed(null);  // back out
                else
                    t.next = node;
            } 
            
            else if (first && spins != 0) {
                --spins;                        // reduce unfairness on rewaits
                Thread.onSpinWait();
            } else if (node.status == 0) {
                node.status = WAITING;          // enable signal and recheck
            } else {
                long nanos;
                spins = postSpins = (byte)((postSpins << 1) | 1);
                if (!timed)
                    LockSupport.park(this);
                else if ((nanos = time - System.nanoTime()) > 0L)
                    LockSupport.parkNanos(this, nanos);
                else
                    break;
                node.clearStatus();
                if ((interrupted |= Thread.interrupted()) && interruptible)
                    break;
            }
        }
        return cancelAcquire(node, interrupted, interruptible);
    }
```

### 2. 释放锁 unlock（）

```java
public void unlock() {
        sync.release(1);
    }
```

release 方法

```java
public final boolean release(int arg) {
        if (tryRelease(arg)) {
            signalNext(head);
            return true;
        }
        return false;
    }
```

ReentrantLock中tryRelease方法：首先判断是否是当前线程加的锁，不是抛出异常

是的话，可重入锁的计数器为0就释放锁，也就是将拥有者设为null

将state设置为state-releases

```java
 protected final boolean tryRelease(int releases) {
            int c = getState() - releases;
            if (getExclusiveOwnerThread() != Thread.currentThread())
                throw new IllegalMonitorStateException();
            boolean free = (c == 0);
            if (free)
                setExclusiveOwnerThread(null);
            setState(c);
            return free;
        }
```

2. 公平锁和非公平锁的区别

```java
//公平锁 
static final class FairSync extends Sync {
        private static final long serialVersionUID = -3000897897090466540L;
        final boolean initialTryLock() {
            Thread current = Thread.currentThread();
            int c = getState();
            if (c == 0) {
                if (!hasQueuedThreads() && compareAndSetState(0, 1)) {
                    setExclusiveOwnerThread(current);
                    return true;
                }
            } else if (getExclusiveOwnerThread() == current) {
                if (++c < 0) // overflow
                    throw new Error("Maximum lock count exceeded");
                setState(c);
                return true;
            }
            return false;
        }

        protected final boolean tryAcquire(int acquires) {
            if (getState() == 0 && !hasQueuedPredecessors() &&
                compareAndSetState(0, acquires)) {
                setExclusiveOwnerThread(Thread.currentThread());
                return true;
            }
            return false;
        }
    }
```

```java
//非公平锁
static final class NonfairSync extends Sync {
        private static final long serialVersionUID = 7316153563782823691L;
        final boolean initialTryLock() {
            Thread current = Thread.currentThread();
            if (compareAndSetState(0, 1)) { // first attempt is unguarded
                setExclusiveOwnerThread(current);
                return true;
            } else if (getExclusiveOwnerThread() == current) {
                int c = getState() + 1;
                if (c < 0) // overflow
                    throw new Error("Maximum lock count exceeded");
                setState(c);
                return true;
            } else
                return false;
        }

        protected final boolean tryAcquire(int acquires) {
            if (getState() == 0 && compareAndSetState(0, acquires)) {
                setExclusiveOwnerThread(Thread.currentThread());
                return true;
            }
            return false;
        }
    }
```

### 3. 公平锁和非公平锁

从源码可以看出，公平锁是在初始获取锁的时候，不是直接使用cas操作试图改变state的值，而是只有在队列为空的时候才会尝试去获取锁，tryAcquire方法，同样是需要先判断是否当前线程是处于第一个等待或者队列是空的。

而非公平锁是直接去使用cas操作改变state的值，成功便直接获得了锁，不需要查看队列的状态