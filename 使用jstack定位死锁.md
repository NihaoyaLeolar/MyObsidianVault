---
tags:
  - 工具
  - 多线程编程
---
# 使用方法
---
```shell
jps             # 获取java进程及其id
jstack <pid>    # 查看对应id
```

# 举例使用
---
写了一个死锁测试程序，模仿哲学家就餐问题，运行后形成了死锁
## 1. 定位到进程id
发现运行的程序县城时 82578
```shell
nihaoya@LittleCats-MacBook-Pro  ~  jps
82577 Launcher
82448 RemoteMavenServer36
82578 TestDeadLock
746
82636 Jps
82414 Main
```

## 2. 查看输出
```shell
jstack 82578
2024-04-07 16:52:09
Full thread dump Java HotSpot(TM) 64-Bit Server VM (25.281-b09 mixed mode):

"Attach Listener" #17 daemon prio=9 os_prio=31 tid=0x00007fee7780d000 nid=0x6803 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"DestroyJavaVM" #16 prio=5 os_prio=31 tid=0x00007fee89090000 nid=0x2203 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"苏格拉底5" #15 prio=5 os_prio=31 tid=0x00007fee87085000 nid=0x6403 waiting for monitor entry [0x000000030f2e2000]
   java.lang.Thread.State: BLOCKED (on object monitor)
	at nihaoya.test.Philosopher.run(TestDeadLock.java:39)
	- waiting to lock <0x000000076ac2afa8> (a nihaoya.test.Chopstick)
	- locked <0x000000076ac2b0a8> (a nihaoya.test.Chopstick)

"苏格拉底4" #14 prio=5 os_prio=31 tid=0x00007fee87084800 nid=0x7103 waiting for monitor entry [0x000000030f1df000]
   java.lang.Thread.State: BLOCKED (on object monitor)
	at nihaoya.test.Philosopher.run(TestDeadLock.java:39)
	- waiting to lock <0x000000076ac2b0a8> (a nihaoya.test.Chopstick)
	- locked <0x000000076ac2b068> (a nihaoya.test.Chopstick)

"苏格拉底3" #13 prio=5 os_prio=31 tid=0x00007fee87083800 nid=0x7303 waiting for monitor entry [0x000000030f0dc000]
   java.lang.Thread.State: BLOCKED (on object monitor)
	at nihaoya.test.Philosopher.run(TestDeadLock.java:39)
	- waiting to lock <0x000000076ac2b068> (a nihaoya.test.Chopstick)
	- locked <0x000000076ac2b028> (a nihaoya.test.Chopstick)

"苏格拉底2" #12 prio=5 os_prio=31 tid=0x00007fee87083000 nid=0x7503 waiting for monitor entry [0x000000030efd9000]
   java.lang.Thread.State: BLOCKED (on object monitor)
	at nihaoya.test.Philosopher.run(TestDeadLock.java:39)
	- waiting to lock <0x000000076ac2b028> (a nihaoya.test.Chopstick)
	- locked <0x000000076ac2afe8> (a nihaoya.test.Chopstick)

"苏格拉底1" #11 prio=5 os_prio=31 tid=0x00007fee87082000 nid=0x6263 waiting for monitor entry [0x000000030eed6000]
   java.lang.Thread.State: BLOCKED (on object monitor)
	at nihaoya.test.Philosopher.run(TestDeadLock.java:39)
	- waiting to lock <0x000000076ac2afe8> (a nihaoya.test.Chopstick)
	- locked <0x000000076ac2afa8> (a nihaoya.test.Chopstick)

"Service Thread" #10 daemon prio=9 os_prio=31 tid=0x00007fee66826000 nid=0x5d0f runnable [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C1 CompilerThread3" #9 daemon prio=9 os_prio=31 tid=0x00007fee77813800 nid=0x5b03 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C2 CompilerThread2" #8 daemon prio=9 os_prio=31 tid=0x00007fee87010800 nid=0x5903 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C2 CompilerThread1" #7 daemon prio=9 os_prio=31 tid=0x00007fee8781b000 nid=0x5703 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C2 CompilerThread0" #6 daemon prio=9 os_prio=31 tid=0x00007fee880c3800 nid=0x7e03 waiting on condition [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Monitor Ctrl-Break" #5 daemon prio=5 os_prio=31 tid=0x00007fee880b7000 nid=0x5547 runnable [0x000000030e7c1000]
   java.lang.Thread.State: RUNNABLE
	at java.net.SocketInputStream.socketRead0(Native Method)
	at java.net.SocketInputStream.socketRead(SocketInputStream.java:116)
	at java.net.SocketInputStream.read(SocketInputStream.java:171)
	at java.net.SocketInputStream.read(SocketInputStream.java:141)
	at sun.nio.cs.StreamDecoder.readBytes(StreamDecoder.java:284)
	at sun.nio.cs.StreamDecoder.implRead(StreamDecoder.java:326)
	at sun.nio.cs.StreamDecoder.read(StreamDecoder.java:178)
	- locked <0x000000076ac8e930> (a java.io.InputStreamReader)
	at java.io.InputStreamReader.read(InputStreamReader.java:184)
	at java.io.BufferedReader.fill(BufferedReader.java:161)
	at java.io.BufferedReader.readLine(BufferedReader.java:324)
	- locked <0x000000076ac8e930> (a java.io.InputStreamReader)
	at java.io.BufferedReader.readLine(BufferedReader.java:389)
	at com.intellij.rt.execution.application.AppMainV2$1.run(AppMainV2.java:53)

"Signal Dispatcher" #4 daemon prio=9 os_prio=31 tid=0x00007fee7780e800 nid=0x4c03 runnable [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Finalizer" #3 daemon prio=8 os_prio=31 tid=0x00007fee7780c000 nid=0x5227 in Object.wait() [0x000000030e4ac000]
   java.lang.Thread.State: WAITING (on object monitor)
	at java.lang.Object.wait(Native Method)
	- waiting on <0x000000076ab08ee0> (a java.lang.ref.ReferenceQueue$Lock)
	at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:144)
	- locked <0x000000076ab08ee0> (a java.lang.ref.ReferenceQueue$Lock)
	at java.lang.ref.ReferenceQueue.remove(ReferenceQueue.java:165)
	at java.lang.ref.Finalizer$FinalizerThread.run(Finalizer.java:216)

"Reference Handler" #2 daemon prio=10 os_prio=31 tid=0x00007fee86812800 nid=0x5423 in Object.wait() [0x000000030e3a9000]
   java.lang.Thread.State: WAITING (on object monitor)
	at java.lang.Object.wait(Native Method)
	- waiting on <0x000000076ab06c00> (a java.lang.ref.Reference$Lock)
	at java.lang.Object.wait(Object.java:502)
	at java.lang.ref.Reference.tryHandlePending(Reference.java:191)
	- locked <0x000000076ab06c00> (a java.lang.ref.Reference$Lock)
	at java.lang.ref.Reference$ReferenceHandler.run(Reference.java:153)

"VM Thread" os_prio=31 tid=0x00007fee87014800 nid=0x3703 runnable

"GC task thread#0 (ParallelGC)" os_prio=31 tid=0x00007fee88016000 nid=0x3d4b runnable

"GC task thread#1 (ParallelGC)" os_prio=31 tid=0x00007fee77008800 nid=0x3c03 runnable

"GC task thread#2 (ParallelGC)" os_prio=31 tid=0x00007fee77009000 nid=0x2e03 runnable

"GC task thread#3 (ParallelGC)" os_prio=31 tid=0x00007fee87808800 nid=0x3a03 runnable

"GC task thread#4 (ParallelGC)" os_prio=31 tid=0x00007fee77808800 nid=0x3903 runnable

"GC task thread#5 (ParallelGC)" os_prio=31 tid=0x00007fee77809000 nid=0x3203 runnable

"GC task thread#6 (ParallelGC)" os_prio=31 tid=0x00007fee77809800 nid=0x3803 runnable

"GC task thread#7 (ParallelGC)" os_prio=31 tid=0x00007fee88017000 nid=0x3503 runnable

"VM Periodic Task Thread" os_prio=31 tid=0x00007fee86820800 nid=0x7853 waiting on condition

JNI global references: 15


Found one Java-level deadlock:
=============================
"苏格拉底5":
  waiting to lock monitor 0x00007fee7703d808 (object 0x000000076ac2afa8, a nihaoya.test.Chopstick),
  which is held by "苏格拉底1"
"苏格拉底1":
  waiting to lock monitor 0x00007fee6680d958 (object 0x000000076ac2afe8, a nihaoya.test.Chopstick),
  which is held by "苏格拉底2"
"苏格拉底2":
  waiting to lock monitor 0x00007fee6680d8a8 (object 0x000000076ac2b028, a nihaoya.test.Chopstick),
  which is held by "苏格拉底3"
"苏格拉底3":
  waiting to lock monitor 0x00007fee7703c208 (object 0x000000076ac2b068, a nihaoya.test.Chopstick),
  which is held by "苏格拉底4"
"苏格拉底4":
  waiting to lock monitor 0x00007fee7703c158 (object 0x000000076ac2b0a8, a nihaoya.test.Chopstick),
  which is held by "苏格拉底5"

Java stack information for the threads listed above:
===================================================
"苏格拉底5":
	at nihaoya.test.Philosopher.run(TestDeadLock.java:39)
	- waiting to lock <0x000000076ac2afa8> (a nihaoya.test.Chopstick)
	- locked <0x000000076ac2b0a8> (a nihaoya.test.Chopstick)
"苏格拉底1":
	at nihaoya.test.Philosopher.run(TestDeadLock.java:39)
	- waiting to lock <0x000000076ac2afe8> (a nihaoya.test.Chopstick)
	- locked <0x000000076ac2afa8> (a nihaoya.test.Chopstick)
"苏格拉底2":
	at nihaoya.test.Philosopher.run(TestDeadLock.java:39)
	- waiting to lock <0x000000076ac2b028> (a nihaoya.test.Chopstick)
	- locked <0x000000076ac2afe8> (a nihaoya.test.Chopstick)
"苏格拉底3":
	at nihaoya.test.Philosopher.run(TestDeadLock.java:39)
	- waiting to lock <0x000000076ac2b068> (a nihaoya.test.Chopstick)
	- locked <0x000000076ac2b028> (a nihaoya.test.Chopstick)
"苏格拉底4":
	at nihaoya.test.Philosopher.run(TestDeadLock.java:39)
	- waiting to lock <0x000000076ac2b0a8> (a nihaoya.test.Chopstick)
	- locked <0x000000076ac2b068> (a nihaoya.test.Chopstick)

Found 1 deadlock.
```