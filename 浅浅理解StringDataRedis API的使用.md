---
tags:
  - springboot
  - 缓存
---
SpringBoot有两个API工具类：`RedisTemplate` 和 它的子类`StringRedisTemplate`
```java
// 默认好了：K为String, V也为String
public class StringRedisTemplate extends RedisTemplate<String, String> 
```
# 首先一个很大的误区
---
并不是存入的值类型为String时我们才考虑用StringRedisTemplate，在实战时，我们对于Hash(Map)存对象、List存列表都用了这个！
所以在我还很不清楚的情况先，暂且可以粗暴理解：**任何时间都使用StringRedisTemplate**，防止Redis可视化出现乱码。（使用RedisTemplate会出现乱码，除非进行配置，但是又因为反射会出现空间浪费）
# 不成体系的理解
---


## 1. 如何使用
**使用 StringRedisTemplate API 存储的时候，对象应该是String类型，或者是String类型组成的！**
- 存入类型是`Strin`g时（可能是对象序列化成了json串/本身就是串），调用opsForValue()，不需要考虑任何
- 存入的类型是`Hash(Map)`，调用的opsForHash()。存入是.putAll(key, map)。需要保证Map中的元素都是String类型，不然会出现类型转换错误，见：[[基于Redis实现短信登录]]中出现的问题。
- 存入的类型是 `List`，待用的opsForList()，存入时有两种。也就是说存入时，可以以此将List中的东西转为JsonStr，或者直接将List转为JsonStr
	- rightPushAll(K,V)：存入的是String
	- rightPushAll(K,Collection\<V\>)：存入的是String组成的集合

## 2. 两层序列化
我感觉这里有**两层**序列化问题，事实就是有以上两个不一样的过程。之前学习时混淆不清，导致理解糊涂！
### 底层序列化
对于 StringRedisTemplate 和 RedisTemplate 在对Redis进行存储时，会调用某些序列化器，**将Java对象转换为字节流(字节数组)** ，以适配在Redis的存储。
- `StringRedisTemplate`：把String（k、v都是这个类型）转换为字节流，需要传入的v是String或者String的集合，这样不会出现自动配置的序列化运行时错误。
- `RedisTemplate`：把k为String，v为Object来转，需要手动配置，不然这个层面出现可视化乱码。
### 表层序列化
这是在业务层面进行的序列化，通常指的是**将 Java 对象转换为 JSON 字符串，或者将 JSON 字符串转换为 Java 对象**。这一层的序列化是为了适应业务需求，将对象转换为文本形式，方便存储或传输。
- `StringRedisTemplate`：需要手动实现，要把k、v变成json串，或者v为json串的集合，取出时要反序列化回去（这里我都用的是hutool包下的JSONUtil工具）
- `RedisTemplate`：直接自动实现（但是为了可视化不出现乱码，手动配以一下序列化器），这是由Java的反射机制实现的（这也就造成了Redis空间浪费，每一条记录里都保存了Object的反射类型唯一名）



## 3. 两个API工具的使用场景
- 目前所学都是简单用`StringRedisTemplate`，轻便好用。**使用时就是都用StringRedisTemplate然后严格按照String去存储！**
- 如果需要更多的灵活性，比如在 Redis 中存储多种类型的对象，或者需要更精细地控制对象的序列化方式，那么 `RedisTemplate` 提供了更多的配置选项。
# 困扰我的问题
---
### 既然都要序列化，为什么Redis还给出了5种值类型，在实现时都是序列化的字节流了，有什么区别？
在以上两层的序列化操作层面，也就是我现在简单实用的存取，好像确实区别不大。可能是为了接口的使用更加通用。（我在学习时一致在学操作，没有学到具体的应用功能，所以思维会出现上面这个问题！）但是它们底层预备的功能强大。比如要用到对应数据结构的特性，去重、排序、阻塞、前插后插等，好像在存时没有区别，其实底层区别很大，对应的功能使用场景差别也很大！