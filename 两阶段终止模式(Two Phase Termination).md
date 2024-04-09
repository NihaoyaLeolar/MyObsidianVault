---
tags:
  - 多线程编程
  - 设计模式
---
>利用打断，实现一个线程优雅地终止另外一个线程。

# 运行流程分析
---
考虑两种打断时机: 线程 **运行时** or **阻塞时**
- 如果在程序阻塞时出现打断，会抛出异常，捕捉异常即可
- 如果程序运行时发生打断，看看打断标记即可
![[CleanShot 2024-03-28 at 18.58.00@2x.png|500]]
# 代码实现
---
```java
class TwoPhaseTermination {  
  
    private Thread monitor;  
  
    //启动监控线程  
    public void start() {  
        monitor = new Thread(() -> {  
            while (true) {  
                Thread current = Thread.currentThread();
      
                if(current.isInterrupted()) {   //打断情况2：运行时被打断 
                    log.debug("料理后事");  
                    break;  
                }  
  
                try {  
	                //模拟线程阻塞
                    sleep(2000);    
                    //模拟线程正常运行
                    log.debug("执行监控记录！");  
                } catch (InterruptedException e) {  //打断情况1：在阻塞时被打断 
                    e.printStackTrace();  
                    //在睡眠时被打断了，重新设置打断标记  
                    current.interrupt();  
                }  
            }  
        });  
  
        monitor.start();  
    }  
  
    //停止监控线程  
    public void stop() {  
        monitor.interrupt();  
    }  
}
```

### 测试

```java
public class Main {  
    public static void main(String[] args) throws InterruptedException {  
        TwoPhaseTermination tpt = new TwoPhaseTermination();  
        
        tpt.start();  
        sleep(5000);  
        tpt.stop();  //打断
    }  
}
```