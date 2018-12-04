---
title: CMU 95702 - Transaction
tags:
  - Distribution-System
categories:
  - Technology
  - Distribution-System
date: 2016-12-2 16:09:45
---
CMU 95702 - Distributed System

Transaction

<!-- more -->

***

# Java synchronized

放一个synchronized的用法的链接: [synchronized的用法](http://www.jianshu.com/p/ea9a482ece5f) 

一个类里面如果有多个synchronized的方法，访问其中一个synchronized方法的时候，别的synchronized方法也是不能访问的。我是不是可以理解成，一个实例反正就一个锁。

下面代码实现一个queue
``` Java 
synchronized first() { 
	if (Q is empty)
		return false
	else 
		remove and return front 
}

synchronized append() { 
	add to rear 
}

```

上面的代码有一个问题，
1. 假设 queue 这时候是空的
2. 设 A 来调用 first() 方法，返回false
3. 这个时候 B 调用了 append()方法，queue 就有东西了
4. 然后 C 来调用 first() 方法，然后 C 把东西取走了

问题就是 A 在第二步先调用的 first() 方法，却被第4步调用的 C 取走了，这就不好了。接下来我们把代码改进一下：

``` Java 
synchronized first() {
	if queue is empty call
		wait()    // 如果queue是空的，释放锁，进入睡眠状态
	
	remove from front 
}
synchronized append() { 
	adds to rear 
	call notify() // 注意这里是notify，只会唤醒，之前wait()的那个一个thread
	
}

```

注意：
notify() : 
1. notify，只会唤醒，之前wait()的那个一个thread
2. notify() 方法其实并没有释放锁，它只是通知之前进入睡眠状态的thread，现在它可以被唤醒了。
3. notify()的这个代码块全部执行完了以后，才会释放锁

notifyall() : 就会唤醒所有在执行了wait()的，等待这个object的的threads

参考 [Work With wait(), notify() and notifyAll() in Java](http://howtodoinjava.com/core-java/multi-threading/how-to-work-with-wait-notify-and-notifyall-in-java/ "Work With wait(), notify() and notifyAll() in Java")

下面一张图来看看 Java进程的状态切换 
![Java线程状态切换](https://sfault-image.b0.upaiyun.com/140/934/1409347274-571e3a2839ea7_articlex "Java线程状态切换") 
图片来自 [Java线程的状态及切换](https://segmentfault.com/a/1190000005006079 "Java线程的状态及切换")

<br>
***
<br>

# Locking to Attain Serializability

r() 是读操作，w()是写操作

r1(x) r2(x) 这两个操作不冲突，所以顺序无所谓
r1(x) w2(x) 这两个操作冲突了，顺序就有所谓了
w1(x) w2(x) 这两个操作也冲突了，顺序就有所谓了
 
我们可以用锁来保证 Serializability

两种锁：
1. readlocks: rL1(x)
2. writelocks: wL2(x).

Before reading,a read lock is set. Before writing, a write lock is set.

A transaction can obtain a lock only if no other transaction has a conflicting lock on the same data item.
这里很关键，注意是 conflicting lock 
1. 我要读一个object，如果另一个线程有这个object的读的锁，没关系不冲突，我可以得到读操作的锁
2. 我要读一个object，如果另一个线程有写的锁，这就冲突了，我现在读不了
3. 我要写一个object，如果另一个线程有读的锁，那么也是冲突的，不能写

Locks may be removed with wU1(y) or rU1(x)


考虑下面的 transaction 的例子：
两个操作是在不同的机器上的，所以对整个操作加 synchronized 是没用的。所以我们只能在服务器端用上的面介绍的锁机制来保证一个好的顺序。
``` 
Client 1 Transaction T;
  // Get write lock on a
  a.withdraw(100);
  // Get write lock on b
  b.deposit(100);
  // Get write lock on c
  c.withdraw(200); 
  b.deposit(200); 
  // Unlock a,b,c

Client 2 Transaction W;
  // Get a read lock on a
  total = a.getBalance();
  // Get a read lock on b
  total = total + b.getBalance();
  // Get a read lock on c
  total = total + c.getBalance();
  // Unlock a,b,1c8
```

下面是一个死锁的例子：
```
Client 1 Transaction T;
// Get a read lock on b
bal = b.getBalance()
// Upgrade to write
b.setBalance(bal*1.1);
Unlock b

Client 2 Transaction W;
// Get a read lock on b
bal = b.getBalance();
// Upgrade to write lock
b.setBalance(bal*1.1);
// Unlock b
```
1. Clinet 1 先拿到读的锁
2. Clinet 2 也拿到读的锁，因为不冲突
3. client 1 读完以后，拿不到写的锁，因为 client 2 在读
4. 同理 client 2 也拿不到写的锁，因为 client 1 在读

<br>
***
<br>

# Two phase locking (2PL)
只有锁也还是不够的，我们还要保证一定的顺序

Serially Equivalent : all pairs of conflicting operations of the two transactions be executed in the same order

给个例子：
!["图1 Conflicts"](http://ogy9sznv2.bkt.clouddn.com/transaction/01.png "图1 Conflicts")

To be serially equivalent:
If r1(x) occurs before w2(x), then w1(y) must occur before r2(y) and
If r1(x) occurs after w2(x), then w1(y) must occur after r2(y).

也就是说顺序呢 
要么是 r1(x) -> w1(y) -> w2(x) -> r2(y)
要么是 w2(x) -> r2(y) -> r1(x) -> w1(y) 

但是如果只有锁的话，就会有↓图2这种情况

!["图2"](http://ogy9sznv2.bkt.clouddn.com/transaction/02.png "图2")
T1 执行到 rU1(x) 转到 T2， T2执行完了再回到 T1 继续执行

也就是说 顺序是 r1(x) -> r2(y) -> w1(y) -> w2(x) 

这个时候我们需要另外一个机制 Two phase locking (2PL):
2PL demands that all locks are obtained for each transaction before releasing any of them.

如↓图3所示，感觉就是 synchronized 的机制很像了。

!["图3 2PL"](http://ogy9sznv2.bkt.clouddn.com/transaction/03.png "图3 2PL")

# Lock_item 和 unlock_item() 方法

```
Lock_Item(x)
  	if(Lock(x) == 0)
  		// 如果这个对象没有锁，那么就把锁拿过来，加上
		Lock(x) = 1 
	else {
		// 如果这个对象现在有锁
		// 那么就等着，直到被叫醒
  		wait until Lock(x) == 0 and
  		we are woken up.
  	GOTO B 
	}
```

```
Unlock_Item(x)
	Lock(x) = 0
	if any transactions are waiting 
	then wake up one of the waiting transactions.
```

死锁 （Deadlock）

Four Requirements for deadlock:
(1) Resources need mutual exclusion. They are not thread safe.
(2) Resources may be reserved while a process is waiting for more. 
(3) Preemption is not allowed. You can't force a process to give up a resource.
(4) Circular wait is possible. X wants what Y has and Y wants what Z has but Z wants what X has.

Solutions (short course):
Prevention (disallow one of the four)
Avoidance (study what is required by all before beginning) Detection (using time-outs or wait-for graphs) and recovery

一个例子
!["图4 Deadlock"](http://ogy9sznv2.bkt.clouddn.com/transaction/04.png "图4 Deadlock")

因为 2PL 要求在释放锁之前是要获得所有的锁的。
比如在 Transaction T 中，执行完对 A 的操作以后，要获得 B 的锁，才能释放 A 的锁，但是 B 的锁在 Transaction U 那里，但是 Transaction U 也不能释放 B 的锁，因为它要先 获得 A 的锁。这样就卡住了。

# Two phase commit (2PC)

!["图5 Two phase commit "](http://ogy9sznv2.bkt.clouddn.com/transaction/05.png "图5 Two phase commit ")

两阶段提交 不想解释了 偷懒………………


***

Over！