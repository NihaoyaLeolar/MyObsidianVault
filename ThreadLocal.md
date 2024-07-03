>ThreadLocal 是 Java 中的一个类
>它提供了一种在多线程环境中**每个线程都有自己独立的副本变量**的机制

每个线程都可以访问和修改自己的 ThreadLocal 变量（一般被称作本地变量），而不会影响其他线程中的该变量。在多线程环境中，可以**更高效**地存储和访问线程本地变量，同时**避免线程安全问题**（因为本身的隔离性）。
- threadlocal变量的生命周期与当前线程相同
- 使用完毕后要清除theadlocal，避免内存泄漏
# 使用方式
---
一般会使用`xxxHolder`来包装 ThreadLocal 要存的内容与相关操作，之后在各地直接调用方法操作，如:
```java
public class UserHolder {  
    //定义一个静态ThreadLocal常量，用来存储UserDTO对象
    private static final ThreadLocal<UserDTO> tl = new ThreadLocal<>();  
    // 封装 setter & getter
    public static void saveUser(UserDTO userDTO){  
        tl.set(userDTO);  
    } 
    public static UserDTO getUser(){  
        return tl.get();  
    }  
    // 使用完毕后，调用remove方法清除变量，避免内存泄漏 
    public static void removeUser(){  
        tl.remove();  
    }  
}
```
上面是在黑马点评项目中，存储登录用户的对象的例子，避免了反复传递userdto对象。还有其他例子，如把从数据库连接池的连接取出来后存入，避免每次都要去池子中拿。

# 核心源码
---
线程存储本地变量（需要共享的对象副本）的设计，涉及到的类有：
- **Thread**：有两个字段，分别是ThreadLocalMap对象、InheritableThreadLocal对象
- **TheadLocal\<T\>**：对外提供本地变量的get和set
	- 内部类 **ThreadLocalMap**：存储一个线程所有的本地变量
	- 子类 **InheritableThreadLocal**：存储一个线程可以被继承的本地变量
### ThreadLocalMap 类
TheadLocal内部类，设计了一套完整的方法，来维护一个**哈希表** -table(Entry数组)，用来**存储一个线程的所有 ThreadLocal-对象 键值对**。
- key：threadlocal\<T> 对象
- value：要保存的对象，T类型的本地变量

可以空构造，插入第一条threadlocal键值对来构造，也可以传入父线程的ThreadLocalMap对象来构造！
```java
static class ThreadLocalMap {
	private Entry[] table;   // Entry是内部定义的类，就是键值对
	// 两种构造函数
	ThreadLocalMap(ThreadLocal<?> firstKey, Object firstValue) {
		...
	}
	private ThreadLocalMap(ThreadLocalMap parentMap) {
		...
	}
}
```
### Thread 类
每一个线程对象中都有相关字段：
- `threadLocals`：本地变量，本线程的ThreadLocalMap对象，在第一次请求存储对象时创建
- `inheritableThreadLocals`：可以被继承的本地变量，来自父线程的InheritableThreadLocalMap的拷贝，也可以是本线程存储的InhertableThreadLocal。构造时通过布尔值 inheritThreadLocals 可以指定是否要继承父线程的 inheritableThreadLocals
```java
public class Thread implements Runnable{
	/* ThreadLocal values pertaining to this thread. This map is maintained  
	 * by the ThreadLocal class. */
	ThreadLocal.ThreadLocalMap threadLocals = null;  
  
	/* InheritableThreadLocal values pertaining to this thread. This map is maintained 
	 * by the InheritableThreadLocal class. */
    ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;
    
	private Thread(ThreadGroup g, Runnable target, String name,  
               long stackSize, AccessControlContext acc,  
               boolean inheritThreadLocals) {
        ...
		if (inheritThreadLocals && parent.inheritableThreadLocals != null)  
			this.inheritableThreadLocals =  
		        ThreadLocal.createInheritedMap(parent.inheritableThreadLocals);
		...
}
```

### ThreadLocal 类
泛型类，标识ThreadLocal需要存储的对象类型。
构造函数只有默认空构造，为线程创建 `ThreadLocalMap` 对象的时机：
- 当线程被创建时，如果父线程允许继承本地变量的，并且父线程有本地变量，那么会调用 `createInheritedMap` 方法创建，将继承到的变量存在本线程的map中（同时也会创建本线程的InheribaleThreadLocal对象）
- 在线程第一次存储对象（通过threadlocal调用set方法）时，如果map还没被创建就会调用 `createMap`方法创建

```java
public class ThreadLocal<T> {
	ThreadLocalMap getMap(Thread t) {  
	    return t.threadLocals;  
	} 
	
	public void set(T value) {  
	    Thread t = Thread.currentThread();  
	    ThreadLocalMap map = getMap(t);  
	    if (map != null) {  
	        map.set(this, value);  
	    } else {  
	        createMap(t, value);  
	    }  
	}
	public T get() {  
	    Thread t = Thread.currentThread();  
	    ThreadLocalMap map = getMap(t);  
	    if (map != null) {  
	        ThreadLocalMap.Entry e = map.getEntry(this);  
	        if (e != null) {  
	            @SuppressWarnings("unchecked")  
	            T result = (T)e.value;  
	            return result;  
	        }  
	    }  
	    return setInitialValue();  
	}
	 
	void createMap(Thread t, T firstValue) {  
	    t.threadLocals = new ThreadLocalMap(this, firstValue);  
	}
	
	// 在子线程创建时，可能会被Thread的构造函数调用
	static ThreadLocalMap createInheritedMap(ThreadLocalMap parentMap) {  
	    return new ThreadLocalMap(parentMap);  
	}
}
```
### InheritableThreadLocal 类
是ThreadLocal的子类，专门用于存储需要继承的ThreadLocals
```java

public class InheritableThreadLocal<T> extends ThreadLocal<T> {  
    public InheritableThreadLocal() {}  
  
    protected T childValue(T parentValue) {  
        return parentValue;  
    }  
    ThreadLocalMap getMap(Thread t) {  
	   return t.inheritableThreadLocals;  
	}
    void createMap(Thread t, T firstValue) {  
        t.inheritableThreadLocals = new ThreadLocalMap(this, firstValue);  
    }  
}
```