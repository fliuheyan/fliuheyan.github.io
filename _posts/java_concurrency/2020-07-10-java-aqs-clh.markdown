---
layout: post
title:  "AQS & CLH"
date:   2020-07-10 11:11:11 +0100
---

## AQS 
> 核心三要素：状态，队列，CAS

对于一个thread来说，想要获得临界区的资源，就是需要在CLH队列中进行排队，直到成功的获得了锁,类似与我们日常生活中的排队行为。
所以对于thread如何正确的获取与释放锁的问题就转为了，一个新的node如何正确的入队和出队。
 
### AQS结构
AQS持有head和tail节点的引用，并且为lazy initialize.只有当第一次调用enq的时候才会初始化
```java
class AbstractQueuedSynchronizer {
    private transient Thread exclusiveOwnerThread; //当前持有锁的线程
    private transient volatile Node head;   
    private transient volatile Node tail;
    private volatile int state; // state = 0 当前锁没有被占用, 
}
```

Node结构
```java
static final class Node {
        /** Marker to indicate a node is waiting in shared mode */
        static final Node SHARED = new Node();
        /** Marker to indicate a node is waiting in exclusive mode */
        static final Node EXCLUSIVE = null;

        /** waitStatus value to indicate thread has cancelled. */
        static final int CANCELLED =  1;
        /** waitStatus value to indicate successor's thread needs unparking. */
        // successor's thread应当被刮起，当前节点release或者cancel时候应该去unparking successor's thread
        static final int SIGNAL    = -1;
        /** waitStatus value to indicate thread is waiting on condition. */
        static final int CONDITION = -2;
        /**
         * waitStatus value to indicate the next acquireShared should
         * unconditionally propagate.
         */
        static final int PROPAGATE = -3;

        /**
         * Status field, taking on only the values:
         *   SIGNAL:     The successor of this node is (or will soon be)
         *               blocked (via park), so the current node must
         *               unpark its successor when it releases or
         *               cancels. To avoid races, acquire methods must
         *               first indicate they need a signal,
         *               then retry the atomic acquire, and then,
         *               on failure, block.
         *   CANCELLED:  This node is cancelled due to timeout or interrupt.
         *               Nodes never leave this state. In particular,
         *               a thread with cancelled node never again blocks.
         *   CONDITION:  This node is currently on a condition queue.
         *               It will not be used as a sync queue node
         *               until transferred, at which time the status
         *               will be set to 0. (Use of this value here has
         *               nothing to do with the other uses of the
         *               field, but simplifies mechanics.)
         *   PROPAGATE:  A releaseShared should be propagated to other
         *               nodes. This is set (for head node only) in
         *               doReleaseShared to ensure propagation
         *               continues, even if other operations have
         *               since intervened.
         *   0:          None of the above
         *
         * The values are arranged numerically to simplify use.
         * Non-negative values mean that a node doesn't need to
         * signal. So, most code doesn't need to check for particular
         * values, just for sign.
         *
         * The field is initialized to 0 for normal sync nodes, and
         * CONDITION for condition nodes.  It is modified using CAS
         * (or when possible, unconditional volatile writes).
         */
        volatile int waitStatus;

        volatile Node prev;

        volatile Node next;

        volatile Thread thread;
        
        // 当前节点的状态，独占的，共享的
        Node nextWaiter;

        final boolean isShared() {
            return nextWaiter == SHARED;
        }

        /** Establishes initial head or SHARED marker. */
        Node() {}

        // VarHandle mechanics
        // 通过varHandle提供CAS方法
        private static final VarHandle NEXT;
        private static final VarHandle PREV;
        private static final VarHandle THREAD;
        private static final VarHandle WAITSTATUS;
        static {
            try {
                MethodHandles.Lookup l = MethodHandles.lookup();
                NEXT = l.findVarHandle(Node.class, "next", Node.class);
                PREV = l.findVarHandle(Node.class, "prev", Node.class);
                THREAD = l.findVarHandle(Node.class, "thread", Thread.class);
                WAITSTATUS = l.findVarHandle(Node.class, "waitStatus", int.class);
            } catch (ReflectiveOperationException e) {
                throw new ExceptionInInitializerError(e);
            }
        }
    }
```

<figure>
	<img src="{{ site.baseurl }}/assets/clh.png" alt="hehe">
	<figcaption>
		 CLH队列.head基本上就是一个dummy node
	</figcaption>
</figure>

### Exclusive lock mode

#### Acquire Exclusive Lock
> 独占锁的获取和释放

独占锁的state只有两种 
* CANCELLED=1 
* SIGNAL=-1


```java
class A {
    private final ReentrantLock lock = new ReentrantLock();
    public void m() {
        lock.lock();
        try {
            
        }finally{
            lock.unlok();
        }
    }
}
```

ReentrantLock#lock实现.
sync是`AbstractQueuedSynchronizer`的子类，实现了fair和NonFair的tryAcquire方法
```shell script
public void lock() {
        sync.acquire(1);
    }
```
 
```shell script
public final void acquire(int arg) {
        //tryAcquire 由子类实现
        if (!tryAcquire(arg) &&
            acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
            selfInterrupt();
}
``` 

* `tryAcquire` 方法由ReentrantLock类实现，分为公平锁和非公平锁两种.
* `addWaiter` 方法作用等同于 `enq`方法。如果tail==null则初始化head & tail，否则将当前Node加到队尾，并且通过CAS设置tail。然后返回当前node
* `acquireQueued` 当前线程自旋，并尝试获得锁。当获得锁之后将AQS的state设置为1(重入锁喜+1)，并将exclusiveOwnerThread设置为当前thread

```shell script
final boolean acquireQueued(final Node node, int arg) {
        boolean interrupted = false;
        try {
                for (;;) {
                    final Node p = node.predecessor();
                    // tryAcquire成功获得锁之后
                    // 会将AQS的state设为1， 
                    if (p == head && tryAcquire(arg)) {
                        //将当前节点设置为head
                        //然后在这里将当前节点的thread和prev.next设置为null
                        setHead(node);
                        p.next = null; // help GC
                        }
                    }
                    return interrupted;
                }
                //找到当前节点的上一个非CANCEL节点，如果该节点的waitStatus为Node.SIGNAL，则返回true
                if (shouldParkAfterFailedAcquire(p, node))
                    interrupted |= parkAndCheckInterrupt();
            }
        } catch (Throwable t) {
            //将当前节点waitStatus设置为Node.CANCELLED;
            //并将当前节点从queue中删除
            cancelAcquire(node);
            if (interrupted)
                selfInterrupt();
            throw t;
        }
    }
```

入队
```shell script
private Node enq(Node node) {
        for (;;) {
            Node oldTail = tail;
            if (oldTail != null) {
                node.setPrevRelaxed(oldTail); //设置当前节点的prev为tail
                if (compareAndSetTail(oldTail, node)) {
                    oldTail.next = node;  //设置oldTail.next
                    return oldTail;
                }
            } else {
                initializeSyncQueue(); //初始化head 和 tail节点. 
            }
        }
    }
```

#### Release Exclusive Lock

```shell script
public void unlock() {
    sync.release(1);
}
```

```shell script
public final boolean release(int arg) {
    if (tryRelease(arg)) {
        Node h = head;
        if (h != null && h.waitStatus != 0)
            unparkSuccessor(h);
        return true;
    }
    return false;
}
```
* `tryRelease`方法主要有ReentrantLock & ReentrantReadWriteLock两种(exclusive | share).
* `unparkSuccessor` 唤醒successor's thread

考虑一下释放锁时候的状态
* state >= 1
* exclusiveOwnerThead为当前thread

```shell script
protected final boolean tryRelease(int releases) {
    int c = getState() - releases;
    if (Thread.currentThread() != getExclusiveOwnerThread())
        throw new IllegalMonitorStateException();
    boolean free = false;
    // 所有的线程都释放了锁
    if (c == 0) {
        free = true;
        setExclusiveOwnerThread(null);
    }
    setState(c);
    return free;
}
```

### Share Lock Mode
Share Lock对应的具体类为`ReentrantReadWriteLock`是`ReentrantLock`的子类

//TODO Write Lock的抢占 

* exclusiveOwnerThead一直都为null，因为共享锁允许多线程同时持有
* exclusive只有在release或者cancel的时候才会unpark successor's thread. share lock当该节点设置为head的时候也会触发unpark

#### Acquire Lock

```shell script
public void lock() {
    sync.acquireShared(1);
}
```

```shell script
public final void acquireShared(int arg) {
    if (tryAcquireShared(arg) < 0)
        doAcquireShared(arg);
}
```

`tryAcquireShared` 方法不分析了，太麻烦了
* `tryAcquireShared` < 0 获取共享锁失败
* `tryAcquireShared` > 0 加读锁成功
* `tryAcquireShared` = 0 加写锁成功

`doAcquireShared`的实现
```shell script
private void doAcquireShared(int arg) {
        final Node node = addWaiter(Node.SHARED);
        boolean interrupted = false;
        try {
            for (;;) {
                final Node p = node.predecessor();
                if (p == head) {
                    int r = tryAcquireShared(arg);
                    if (r >= 0) {
                        setHeadAndPropagate(node, r);
                        p.next = null; // help GC
                        return;
                    }
                }
                if (shouldParkAfterFailedAcquire(p, node))
                    interrupted |= parkAndCheckInterrupt();
            }
        } catch (Throwable t) {
            cancelAcquire(node);
            throw t;
        } finally {
            if (interrupted)
                selfInterrupt();
        }
    }
```

其中与exclusive lock不同的点:
* `addWaiter(Node.EXCLUSIVE)` => `final Node node = addWaiter(Node.SHARED);` 
* `tryAcquire(arg)` => `int r = tryAcquireShared(arg);`
* `setHead(node)` => `setHeadAndPropagate(node, r);`








出队
```
```

unparkSuccessor

```
Acquire:
     while (!tryAcquire(arg)) {
        <em>enqueue thread if it is not already queued</em>;
        <em>possibly block current thread</em>;
     }
```
 
 ```
 Release:
     if (tryRelease(arg))
        unblock the first queued thread;
```

通过一个FIFO wait queue进行同步状态管理

### CLH
CLH means Craig, Landin, and Hagersten
CLH队列是一个双向的FIFO链表







1. 处理 timeout 和 interrupts
2. 可重入(shared lock)



```
        --------        --------         -------- 
head    | prev |  <---  | prev  |       | prev  |    tail
        | next |  --->  | next  |       | next  |
        -------         --------        ---------
```

Node的数据结构
```java
class Node {
    volatile Thread thread; //入队线程
    volatile int waitStatus; // indicates whether the thread should be blocked
    volatile Node next;
    Node nextWaiter; // ????
    
}
```

>>> 初始状态的CLH队列需要一个dummy head node

#### enqueue
设置tail node的next结点到新的node，由CAS操作来保证入队的原子性.


#### dequeue
从队列中删除当前节点，同时唤醒successor. 同时在唤醒successor的时候要避免有多个新的节点指向当前节点的next，
而发生的竞争。


#### CAS

There are 3 parameters for a CAS operation:

1. old value
2. new value
3. old value在内存中的偏移地址 => 内存值

1.8 之前是Unsafe, 1.8之后是VarHandle

`ABA`的问题


### 总结

为什么很多时候遍历CLH队列的时候是从尾部开始做遍历
```shell script
    node.setPrevRelaxed(oldTail);  //1
    if (compareAndSetTail(oldTail, node)) { //2
        oldTail.next = node; //3
        return oldTail;
}
```
这里是一个新的node加入CLH queue的代码。

### 参考

https://segmentfault.com/a/1190000015739343
