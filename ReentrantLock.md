---
tags:
  - 多线程编程
  - 锁
---
与[[Synchronized 对象锁]]一样，都是[[可重入锁]]。但相比之下，ReentrantLock 有如下不同点：
- 可以中断
- 可以设置超时时间
- 可以设置为公平锁（先进先出）：默认是不公平的
- 支持多个条件变量：synchronized的WaitSet只有一个，但是ReentrantLock可以对WaitSet细分
# 基本语法
---
## 需要new一个ReentrantLock对象
```java
private static ReentrantLock reentrantLock = new ReentrantLock();
```
## 通过锁对象操作
```java
// 1. 获取锁（放在try里面第一行也一样），有几种方法 
reentrantLock.lock();               //获得锁，失败了就进入阻塞队列等待
reentrantLock.lockInterruptibly();  //获取锁，失败了就进入阻塞队列等待，并设置成等待时是打断的
reentrantLock.tryLock();            //尝试获取锁：失败立即返回false而不等待；成功返回true而获取到锁
//尝试获取锁：失败等待设定时间，期间可以被打断；成功返回true并获取到锁
reentrantLocktryLock(long timeout, TimeUnit unit)               

// 2. 使用锁
try {
	// 临界区代码...
} finally {
	// 3. 释放锁
	reentrantLock.unlock(); 
}
```

# 基本特性
---
## 1. 可重入
在[[可重入锁]]中有演示

## 2. 可打断
可以终止线程的阻塞等待，避免死锁的发生
```java
public class TestReentrantLock {  
    private static ReentrantLock lock = new ReentrantLock();  
    public static void main(String[] args) {  
        Thread t1 = new Thread(() -> {  
            try {  
                //如果没有竞争，那么此方法就会拿到lock对象锁  
                //如果有竞争就会进入阻塞队列，可以被其他线程用interrupt方法打断  
                log.debug("尝试获得锁");  
                lock.lockInterruptibly();  //获取锁，并设置成等待时是打断的  
            } catch (InterruptedException e) {  
                e.printStackTrace();  
                log.debug("没有获得锁，返回");  
                return;  
            }  
  
            try {  
                log.debug("获取到锁了");  
            } finally {  
                lock.unlock();  
            }  
        }, "t1");  
  
        lock.lock();  //主线程先获得了锁  
        t1.start();  
  
        Sleeper.sleep(1);  
  
        log.debug("打断t1");  
        t1.interrupt();  
    }  
}
```
![[CleanShot 2024-04-16 at 16.37.47@2x.png]]
## 3. 锁超时
使用 `tryLock()` 方式获取锁，并设置超时时间。可以避免死锁
```java
//尝试获取锁：失败立即返回false而不等待；成功返回true而获取到锁
reentrantLock.tryLock(); 
//尝试获取锁：失败等待设定时间，期间可以被打断；成功返回true并获取到锁
reentrantLock.tryLock(long timeout, TimeUnit unit)     
```

```java
public class TestReentrantLock {  
    private static ReentrantLock lock = new ReentrantLock();  
    public static void main(String[] args) {  
        Thread t1 = new Thread(() -> {  
            log.debug("尝试获得锁");  
  
            //tryLock有两种模式，带参数or不带参数  
            //尝试获得锁，直接返回是否成功（bool值）  
            //if (!lock.tryLock())  
  
            //尝试获得锁，等待1秒钟（也支持可打断的特性），会返回是否成功（bool值）  
            try {  
                if (!lock.tryLock(1, TimeUnit.SECONDS)) {  
                    //获取锁失败  
                    log.debug("获取不到锁");  
                    return;  
                }  
            } catch (InterruptedException e) {  
                e.printStackTrace();  
                log.debug("获取不到锁");  
                return;  
            }  
  
            try {  
                log.debug("获取到了锁");  
            } finally {  
                lock.unlock();  
            }  
  
        }, "t1");  
  
        //主线程先获取锁  
        lock.lock();  
        log.debug("获取到了锁");  
  
        t1.start();  
  
        Sleeper.sleep(1);  
        lock.unlock();  
        log.debug("释放了锁");  
  
    }  
}
```

## 4. 可设置成公平锁
公平锁本意可以用于解决[[饥饿]]问题，但是没有必要...因为有其他方法解决饥饿，**公平锁会降低并发度**！
直接看源码，不好复现。ReentrantLock构造函数：
```java
public ReentrantLock() {  
    sync = new NonfairSync();  
}  
  
public ReentrantLock(boolean fair) {  
    sync = fair ? new FairSync() : new NonfairSync();  
}
```

## 5. 可设置多个条件变量
- [[Synchronized 对象锁]] 中也有条件变量，就是 waitSet 休息室，当条件不满足时进入 waitSet 等待
- ReentrantLock 的条件变量比 synchronized 强大之处在于，它是支持多个条件变量的。

这就好比synchronized 是那些不满足条件的线程都在一间休息室等消息。而 ReentrantLock 支持多间休息室，有专门等烟的休息室、专门等早餐的休息室、唤醒时也是按休息室来唤 醒
### 使用方式
```java
//创建锁对象
private static ReentrantLock lock = new ReentrantLock();
//创建多个条件变量，加到锁上
Condition condition1 = lock.newCondition();  
Condition condition2 = lock.newCondition();  
```
和[[Synchronized 对象锁]]中的[[wait()与notify()]]方法很像，这里也采用 await()与signal()、signalAll()方法

- 线程1
```java
//await 前需要获得锁  
lock.lock();  
  
//await 执行后，会释放锁，进入条件变量的WaitSet等待  
condition1.await();  
//竞争 lock 锁成功后，从 await 后继续执行  
```
- 线程2
```java
// await 的线程被唤醒（或打断、或超时）取重新竞争 lock 锁  
condition1.signal();  
condition1.signalAll();
```

# 实践
---
[[使用ReentrantLock解决死锁问题]]
