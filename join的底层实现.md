直接上源码，通过wait实现的，代码思想是基于[[保护性暂停模式(Guarded Suspension)]]
```java
//Thread.java

public final void join() throws InterruptedException {  
    join(0);  
}

public final synchronized void join(long millis) throws InterruptedException {  
    long base = System.currentTimeMillis();  
    long now = 0;  
  
    if (millis < 0) {  
        throw new IllegalArgumentException("timeout value is negative");  
    }  
  
    if (millis == 0) {  
	    // 调用者线程进入本线程的waitSet等待, 直到本线程运行结束 or 超时
        while (isAlive()) {  
            wait(0);  
        }  
    } else {  
        while (isAlive()) {  
            long delay = millis - now;  
            if (delay <= 0) {  
                break;  
            }  
            wait(delay);  
            now = System.currentTimeMillis() - base;  
        }  
    }  
}
```