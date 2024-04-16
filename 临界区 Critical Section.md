---
tags:
  - 多线程编程
---
>一段代码内，存在对**共享资源**的**多线程读写**操作，称这段代码为临界区

例如：
```java
static int counter = 0;  //共享资源

static void increment() 
// 临界区 
{ 
	counter++; 
}

static void decrement() 
// 临界区 
{ 
	counter--; 
}
```