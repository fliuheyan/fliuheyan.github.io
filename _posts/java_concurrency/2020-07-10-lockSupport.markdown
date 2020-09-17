---
layout: post
title:  "AQS & CLH"
date:   2020-07-10 11:11:11 +0100
---

## Lock Support


挂起当前线程,唤醒：
* 其他线程调用`unpark`
* `Thread#interrupt`
```
public static void park(Object blocker) {
        Thread t = Thread.currentThread();
        setBlocker(t, blocker);
        U.park(false, 0L);
        setBlocker(t, null);
    }
```
