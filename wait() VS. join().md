---
tags:
  - 多线程编程
---
[[wait()与notify()]]
[[线程常见方法剖析]]之join
# 相关性
---
1. 都会让线程进入等待，也就是 **TIMED_WAITING 阻塞状态**，让出cpu时间片
2. wait是[[join的底层实现]]原理，源码的思想是来自于[[保护性暂停模式(Guarded Suspension)]]

# 差别
---
- wait是等待其他线程的某一个资源，join是等待某个线程结束
- wait是Object类的方法，而sleep是Thread类的方法
- wait需要和synchronized对象锁一起用，join没有要求