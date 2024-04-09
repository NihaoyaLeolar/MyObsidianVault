---
tags:
  - 缓存
  - HEAD
---

本文是Redis基础入门学习的自我凝练，讲义：[01.快速入门.md](<file:////Users/nihaoya/Documents/秋招/redis学习/Redis-笔记资料/01-入门篇/讲义/Redis注释版/01.快速入门.md>)

>   Redis[**R**emote **D**ictionary **S**erver 远程词典服务器]诞生于2009年，是一个基于内存的键值型 NoSQL数据库。很火的缓存中间件！是一种 [[键值型数据库]]！

- 学习方式：理解redis中各种数据结构以及使用场景，看看redis底层源码如何实现的
- 安装方式：在官网下载压缩包，解压、编译（准备好gcc环境）、安装即可，然后要改一下配置文件，开机自动后台启动
- 可视化方式：
	- 命令行客户端：redis安装时，已经有了的`redis-cli`命令，可以在命令行通过指令查看
	- 图形化桌面客户端：通过非官方写的可视化App，建立与redis的连接，从而可视化内容
# 特征
----
- 键值(key-value)型，value支持多种不同数据结构
- 功能丰富单线程，每个命令具备原子性
- 低延迟，速度快（基于内存、10多路复用、良好的编码）。
- 支持数据持久化
- 支持主从集群、分片集群
- 支持多语言客户端

# Key Value
---
Redis是典型的key-value数据库，key一般是字符串，而value包含很多不同的数据类型。
#### Key
key一般使用id来作唯一标识，但是不同表的id可能重复，所以Redis的key允许有**多个单词形成层级结构**，多个单词之间用':'隔开。建议使用如下格式：
```	
[项目名]:[业务名]:[类型]:[id]
```

#### Value
![[CleanShot 2024-02-05 at 21.03.42@2x.png]]
# 常见命令
---
入门redis需要了解Redis常见命令，一些常用的不区分类型的叫通用命令，其他不同类型的命令被分成不同的group。命令在[官方文档](https://redis.io/commands)有详细的描述，也可以在命令行使用`help @[group类型]`命令来快速查看。

# Redis 的 Java 客户端
----
redis有很多开源客户端，官方目前推荐的java客户端有三种：
- Jedis：API函数名和redis命令基本一致
- Luttuce：API函数名和java函数操作基本一致
- Redisson：高性能分布式应用场景（暂时不去了解）

Spring框架也对Redis进行了集成 ———— **SpringDataRedis**（spring对redis的集成模块）
- 提供了对不同Redis客户端的整合（Lettuce和Jedis）
- 提供了**RedisTemplate组件**统一API来操作Redis
![[CleanShot 2024-02-07 at 16.00.27@2x.png]]
## 初步实践Jedis
过程：创建maven项目，引入依赖，使用线程池获取Jedis对象，建立连接，即可直接使用。
代码：[jedis-demo](<file:////Users/nihaoya/Documents/秋招/redis学习/Coding/jedis-demo>)
### 初步实践Luttuce
SpringDataRedis默认配置的是使用Luttuce
过程：创建SpringBoot项目，引入依赖，编写yaml配置文件，注入RedisTemplate，直接使用API。
代码：[spring-data-redis-demo](<file:////Users/nihaoya/Documents/秋招/redis学习/Coding/spring-data-redis-demo>)

#### 序列化问题
存入后，redis会出现乱码，key和value所见的可视化不好
阅读源码，RedisTemplate默认使用JDK自带的序列化工具，全都将Object序列化为字节形式，序列化后不是我们想要的。
![[CleanShot 2024-02-07 at 17.26.17@2x.png]]
有两种方法结局，我们一般采用第二种：
1. 使用JSON来序列化：通过**配置bean组件**来自定义Redistemplate的序列化方式。但是由于是使用的Java反射原理实现的自动序列化，所以Redis每一个键值都会存入额外的@class信息，浪费内存。
2. 使用String来序列化：每次存入和取出都手动序列化和反序列化。**StringRedisTemplate**（Redistemplate子类）已经配置好了序列化方式，所以可以不用手动配置组件了。


