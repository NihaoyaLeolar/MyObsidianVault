---
tags:
  - 多线程编程
---
它们是`java.util.concurrent.locks`包下**LockSupport类**中的方法
>让线程进入阻塞/唤醒阻塞

- park方法: 让当前线程阻塞，直到调用unpark方法或者被中断才会继续执行。
- unpark方法: 唤醒指定线程，使其可以继续执行。

注意：如果 unpark 方法在 park 方法之前调用，那么 park 方法会立即返回
# 使用方式

```java
// 暂停当前线程 
LockSupport.park();

// 恢复某个线程的运行 
LockSupport.unpark(threadObj)
```

# [[wait() VS. park()]]
# [[park & unpark 的底层实现]]