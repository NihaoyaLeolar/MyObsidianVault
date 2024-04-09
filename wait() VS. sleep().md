# 相似性
---
两者都会让线程进入睡眠，即 **TIMED_WAITING 阻塞状态**，让出cpu时间片。

# 差别
---
- wait是Object类的方法，而sleep是Thread类的静态方法
- wait需要和synchronized对象锁一起用，sleep没有要求
- wait让线程进入睡眠时，会释放对象锁。而sleep让线程进入睡眠时，不会释放对象锁（如果有）