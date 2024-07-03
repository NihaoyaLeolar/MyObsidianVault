---
tags:
  - 多线程编程
---
在[[重量级锁]]monitor的Owner线程中，通过锁对象调用！
可以用来解决同步问题，详见：[[顺序控制的多种解决方式]]
# wait()
---
因为条件不足，让线程进入waiting态等待！
- `obj.wait()`：让当前的Owner线程进入Waiting状态，加入WaitSet。**无限时等待**：不被调用notify唤醒则一直waiting下去
- `obj.wait(long n)`：让当前的Owner线程进入Waiting状态。**有限时等待**：在 n 毫秒之后结束等待，会被唤醒

### [[wait() VS. sleep()]]
### [[wait() VS. join()]]
## [[wait() VS. park()]]
# Notify()
---
- `obj.notify()`：随机唤醒WaitSet中的一个线程。可能会导致**虚假唤醒**（被唤醒的线程不是理想的，可能还是没有足够的条件）
- `obj.notifyAll()`：唤醒WaitSet中的所有线程。可以配合**while循环**解决虚假唤醒的问题！

# 正确使用姿势
---
这里的使用方式对应一种设计模式：[[保护性暂停模式(Guarded Suspension)]]
```java
//某线程
synchronized(lock) {
	while(条件不足) {
		lock.wait();
	}
	// 干活
}

//另一个线程
synchronized(lock) {
	lock.notifyAll();
}
```