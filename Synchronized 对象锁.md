---
tags:
  - 多线程编程
  - 锁
---
>Java 中用于实现**线程同步**的关键字，俗称 “ **对象锁** ”
>它可以用来控制多个线程对共享资源的访问，确保在同一时刻只有一个线程可以执行被 synchronized 修饰的代码块或方法。

# 原理
----
采用互斥的方式，让**同一时刻至多只有一个线程能持有对象锁**，其它线程再想获取这个对象锁时就会阻塞住。这样就能保证拥有锁的线程可以安全的执行临界区内的代码，不用担心线程上下文切换！

**本质：锁住的其实是对象对应的 Monitor**

>用对象锁保证**临界区内代码的原子性**，临界区内的代码对外是不可分割的，不会被线程切换所打断。
# 实践
----

## 语法
将对象锁把临界区代码框住。对于冲突的临界区，一定是要给同一个对象加锁！
```java
synchronized(非空对象) {
	//临界区代码
}
```
其实还有其他变种写法 ———— **将synchronized加在方法上**！包括成员方法、静态方法
但是本质不要忘了：依旧是对象锁！看清楚锁住的对象是谁就好了！
```java
class Test{
	public synchronized void test() {
		//临界区
	}
}
//等价于
class Test{
	public void test() {
		synchronized(this) {
			//临界区
		}
	}
}
```
```java
class Test{
	public synchronized static void test() {
		//临界区
	}
}
//等价于
class Test{
	public void test() {
		synchronized(Test.java) {
			//临界区
		}
	}
}
```

## 解决[[多线程访问共享资源的线程安全问题]]
![[CleanShot 2024-03-29 at 15.16.41@2x.png|480]]
#### 实现代码
主要是面向过程编写的，先初步了解一下synchronized的使用场景和逻辑
```java
private static int counter = 0;  
private final static Object lock = new Object();  
  
public static void main(String[] args) throws InterruptedException {  
    Thread t1 = new Thread(() -> {  
        for(int i = 0; i < 5000; i++) {  
	        //在临界区使用lock对象锁
            synchronized (lock) {   
                counter++;  
            }  
        }  
    }, "t1");  
  
    Thread t2 = new Thread(() -> {  
        for(int i = 0; i < 5000; i++) {  
	        //在临界区使用lock对象锁
            synchronized (lock) {  
                counter--;  
            }  
        }  
    }, "t2");  
  
    t1.start();  
    t2.start();  
    t1.join();  
    t2.join();  
  
    log.debug("{}",counter);  
}
```

#### 实现代码改进
针对以上代码，做面向对象的封装，可以看看，更加成熟的编码方式：
```java
public class Main {  
  
    public static void main(String[] args) throws InterruptedException {  
  
        Room room = new Room();  
  
        Thread t1 = new Thread(() -> {  
            for (int i = 0; i < 5000; i++) {  
                room.increment();  
            }  
        }, "t1");  
  
        Thread t2 = new Thread(() -> {  
            for (int i = 0; i < 5000; i++) {  
                room.decrement();  
            }  
        }, "t2");  
  
        t1.start();  
        t2.start();  
        t1.join();  
        t2.join();  
  
        log.debug("{}", room.getCounter());  
    }  
}  
  
class Room {  
    private int counter = 0;  
  
    public void increment() {  
        synchronized (this) {  
            counter++;  
        }  
    }  
  
    public void decrement() {  
        synchronized (this) {  
            counter--;  
        }  
    }  
  
    public int getCounter() {  
        synchronized (this) {  
            return counter;  
        }  
    }  
}
```
## 使用synchronized的注意事项
以上面的例子来说：
1. 保证**锁的粒度**最小，在需要保证**原子操作**的地方加锁。例如：把上面的synchronized放在for循环外面，就是不稳妥的
2. 对于共享变量的临界区，**锁对象需要一致**。例如：在上面不使用一样的synchronized (lock)，而是synchronized (lock1)与synchronized (lock2)，则不会生效！
3. 临界区的地方全要加上锁，不能有的加有的不加！例如：在t1加synchronized (lock)而t2不加，则不会生效！
# 注意，它可以实现互斥与同步
----
虽然 java 中互斥和同步都可以采用 synchronized 关键字来完成，但它们还是有区别的：
- 互斥：是阻止临界区的竞态条件发生，同一时刻只能有一个线程执行临界区代码
- 同步：是由于线程执行的先后顺序不同，需要一个线程等待其它线程运行到某个点