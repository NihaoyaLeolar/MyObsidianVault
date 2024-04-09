---
tags:
  - java
---
# 疑问
---
今日看队列实现的源码时的困惑：extends不是继承吗？可是可以继承[[接口]]？不应该是实现[[接口]]吗
```java
public interface Queue<E> extends Collection<E>
```

这里暴露了我对继承和实现的理解不够，下面总结一下：
# 🌟 继承 —— extends
----
在Java中，`extends` 关键字用于接口的继承确实会引起一些混淆，因为它在类继承中也被使用。但是，当用于接口时，它的含义有所不同：
#### 1. 类继承（Class Inheritance）
当一个类使用 `extends` 来继承另一个类时，它获得了父类的所有属性和方法 ———— 表示“扩展”或“继承”父类的功能。
#### 2. 接口继承（Interface Inheritance）
当一个接口使用 `extends` 来继承另一个接口时，它实际上是在**扩展**接口的契约 ———— 表示新接口将包含被继承接口的所有方法签名，并且可以添加更多的方法签名。

# 🌟 实现 —— implements
----
在Java中，类使用 `implements` 关键字来实现（implement）接口。
只有类可以实现接口，接口是不可以使用implements关键字的！


# 总结
---
- **类** 可以实现（implement）一个或多个接口。实现接口意味着类必须提供那些接口声明的所有方法的具体实现。
- **接口** 只能继承（extend）其他接口，而不能实现它们。当一个接口继承另一个接口时，它继承了父接口的所有方法签名。接口可以继承多个其他接口，从而合并这些接口的方法签名。



# 解决疑问
---
所以，当看到 
```java
public interface Queue<E> extends Collection<E>
```
这意味着 `Queue` 接口包含了 `Collection` 接口的所有方法，并可能添加一些特定于队列的方法。