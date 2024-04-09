---
tags:
  - 多线程编程
---
每个线程都有自己的一个 Parker 对象，包含三个字段：
- **_mutex**:
- **_cond**:
- **_counter**: 0为耗尽、1为充足

# 举例理解
---
	好比于：线程就是一辆小车，_counter是油量。初始时，线程运行，油量变为 0
- park相当于停车检查油量，如果为0，就停车
- unpark相当于加油，counter置为1
# 原理解析
---
应该是信号量相关，这里只是了解了过程原理，底层原理还是不够深入！
## 情况1: 先park再unpark
当前线程调用 `Unsafe.park()` 方法:
1. 检查_counter，本情况为为0,获得_mutex互斥锁
2. 线程进入_cond条件变量阻塞
3. 设置_counter=0
![[CleanShot 2024-04-03 at 17.22.42@2x.png|400]]

调用 `Unsafe.unpark(threadObj)` 方法:
1. 设置_counter为1
2. 唤醒_cond条件变量中的 threadObj，线程恢复恢复运行
3. 设置_counter为0
![[CleanShot 2024-04-03 at 17.23.35@2x.png|400]]
## 情况2: 先unpark再park
调用`Unsafe.unpark(threadObj)`方法:
1. 设置_counter为1

当前线程调用`Unsafe.park()`方法:
1. 检查_counter，本情况为1，这时线程无需阻塞，继续运行
2. 设置_counter为0
![[CleanShot 2024-04-03 at 17.28.15@2x.png|400]]