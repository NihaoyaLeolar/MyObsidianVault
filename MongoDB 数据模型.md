# MongoDB 数据模型的概念
---
- `文档（Document）`：单条数据，对应一个数据实体
- `文档集合（Collection）`：多个 document 构成的数据表
- `数据库（Database）`：多个 collection 组成

![[CleanShot 2024-07-08 at 09.12.29@2x.png|600]]

# 通过 Java 访问MongoDB
---
1. 使用 MongoClient 连接并访问 MongoDB 服务器（MongoClient唯一的）
2. 通过 MongoClient 获取MongoDatabase 对象，然后获取文档集合，对文档进行 CRUD 操作

# 操作数据模型
---
### 1. Spring 项目与 MongoDB 的关联方式
项目启动前，数据库已经启动，且通过 MongoClient 关联。MongoClient 会通过 getDatabase方法自动打开配置中指定的 MongoDatabase。也就是说，项目里的数据库操作均是在这个指定的 db 中。

具体的细节 spring 框架已经帮我们封装和隐藏好了，我们只需要启动服务、配置参数...

### 2. 数据访问的流程
**项目与数据库的根本关联在DAO层，也就是repository接口**，其作用：
- 实现 java 中数据对象与数据库中的表相互映射
- 可以对数据表进行CRUD，增删改查的原子操作

当前端传递一个信息给后端，需要对某一数据表的数据进行 CRUD。基本流程如下：
1. controller 接收到前端的信息，分发给 server 层来实现业务
2. server 层使用 dao 层中对应该数据实体的 repository，直接调用合适的方法（该方法可以是该repo中新定义的，也可以是从mongoRepo中继承而来的。）
3. 进入repository后，会在之前指定的 db 中，通过 getcollection 方法获取对应数据表的引用，从而进行真正的 CRUD 

 ### 初始时，数据库/数据表如何创建？
- 在启动数据库服务时，如果用 -dbpath 指定了数据库路径，则会打开指定数据库，如果没有就默认创建一个空数据库。（也可以在项目配置文件中指定数据库路径）
- 在进行第一次数据库访问的操作时，如果对应数据库没有被创建过，则直接创建一个空数据库，然后再访问。当访问到某一个数据表时，如果其不存在则会自动创建一个空表，再执行操作。

综上，也就是说**使用 springboot 项目的 mongoDB 不需要自己创建数据库、数据表，只需要编写好数据编程模型即可！**