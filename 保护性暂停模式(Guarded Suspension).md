---
tags:
  - 多线程编程
  - 设计模式
---
>**Guarded Suspension**: 用在一个线程等待另一个线程的执行结果。
>因为要等待另一方的结果，是一种**同步模式**，采用这种设计模式生产和消费**解耦**。

1. 有一个结果需要从一个线程传递到另一个线程，让他们**关联同一个 GuardedObject**.(如果有结果不断从一个线程到另一个线程那么可以使用消息队列 ———— 见生产者/消费者)
2. JDK 中，join 的实现、Future 的实现，采用的就是此模式 
![[CleanShot 2024-04-02 at 15.28.34@2x.png|600]]

# 实现 GuardedObject 类  ———— 单任务版
---
这里只有一个任务，也就是某一个线程等待另一个线程，只需要一个中间对象(GuardedObject)
## 基本版
```java
class GuardedObject {  
    private int id;
    //结果  
    private Object response;  

	public GuardedObject(int id) {  
	    this.id = id;  
	}
  
    //获取结果  
    public Object get() {  
        synchronized (this) {  
            //还没有结果  
            while(response == null) { //循环防止被虚假唤醒  
                try {  
                    this.wait();  
                } catch (InterruptedException e) {  
                    throw new RuntimeException(e);  
                }  
            }  
        }  
        return response;  
    }  
  
    //产生结果  
    public void complete(Object response) {  
        synchronized (this) {  
            this.response = response;  
            this.notifyAll();  
        }  
    }  
}
```

## 超时限制版本
这里重载一个get方法即可
```java
public Object get(long timeout) {  
    synchronized (this) {  
        //开始时间  
        long start = System.currentTimeMillis();  
        //经历的时间  
        long passedTime = 0;  
  
        //等待结果，最多等timeout毫秒，超时直接退出，返回null  
        while(response == null) { //循环防止被虚假唤醒  
  
            //这一轮等待最多等待的时间  
            long waitTime = timeout - passedTime;  
            if(waitTime <= 0) {  
                break;  
            }  
  
            try {  
                this.wait(waitTime);  //中间可能出现虚假唤醒，这样可以让等待的时间最小  
            } catch (InterruptedException e) {  
                throw new RuntimeException(e);  
            }  
  
            passedTime = System.currentTimeMillis() - start;  
  
        }  
    }  
    return response;  
}
```

## 测试/应用场景
一般会这样使用上面的代码
```java
GuardedObject guardedObject = new GuardedObject();  
  
//线程1等待线程2的结果  
new Thread(() ->{  
    //等待结果  
    log.debug("等待结果");  
    Object obj = guardedObject.get(9000);  
    log.debug("拿到结果:{}", obj);  
}, "t1").start();  
  
//线程2计算结果  
new Thread(() -> {  
    log.debug("正在计算结果...");  
    sleep(10); //实际上的操作可能比较耗时  
    guardedObject.complete("Hello, Guard Suspension!");  
}, "t2").start();
```

# 实现 MailBox 类 ———— 多任务版
---
当出现多对等待(一个线程等待另一个线程)，每一对就需要一个中间对象(GuardedObject)。这里就可以比如：很多个收件人，等待专一的某个邮递员，来送信，需要一个信箱统一管理两人之间的关联。

所以需要新加一个中间类 ———— 这里取名为`MailBoxes`，来管理多个GuardedObject！
![[CleanShot 2024-04-02 at 17.55.22@2x.png|580]]
注意这里是一对一wait模式，如果生产方和消费方不是一一对应的，那么使用[[生产者消费者模式]]！
## 核心代码
经过测试，这里使用Hashtable依旧会有出现线程安全问题。但是ConcurrentHashMap好像不会。
原因应该是出现在模拟送信的那里。估计是多个操作叠加是不安全的！
怎么解决、原因探讨，先放一放！
```java
class Mailboxes {  
    private static Map<Integer, GuardedObject> boxes = new Hashtable<>();  
    // private static Map<Integer, GuardedObject> boxes = new ConcurrentHashMap<>();  
  
    private static int count = 1;  
  
    private static synchronized int generateId() {  
        return count++;  
    }  
  
    public static GuardedObject createGuardedObj() {  
        GuardedObject guardedObject = new GuardedObject(generateId());  
        boxes.put(guardedObject.getId(), guardedObject);  //存档  
        return guardedObject;  
    }  
  
    public static GuardedObject getGuardedObj(int id) {  
//        return boxes.get(id);  
        return boxes.remove(id); //返回的同时会删除  
    }  
  
    public static Set<Integer> getIds() {  
        return boxes.keySet();  
    }  
}
```

## 测试/应用场景
根据业务要求来使用。
这里将收件人和邮递员都设置成线程类：
```java
class People extends Thread {  
    @Override  
    public void run() {  
        //收信  
        GuardedObject guardedObject = Mailboxes.createGuardedObj();  
        log.debug("开始收信 id：{}", guardedObject.getId());  
        Object mail = guardedObject.get(50000);  
        log.debug("收到信了 id：{}, 内容:{}", guardedObject.getId(), mail);  
    }  
}
```

```java
class PostMan extends Thread {  
  
    private final int id;  
    private final String mail;  
  
    public PostMan(int id, String mail) {  
        this.id = id;  
        this.mail = mail;  
    }  
  
    @Override  
    public void run() {  
        //发信  
        GuardedObject guardedObject = Mailboxes.getGuardedObj(id);  
        log.debug("送信 id:{}", guardedObject.getId());  
        guardedObject.complete(mail);  
    }  
}
```
模拟收发信：
```java
//模拟收信  
for (int i = 0; i < 3; i++) {  
    new People().start();  
}  
  
sleep(1);  
  
//模拟送信  
for (int id : Mailboxes.getIds()) {  
    new PostMan(id, "Dear: Hello! Here your are! ID" + id).start();  
}
```