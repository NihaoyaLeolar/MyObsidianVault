---
tags:
  - MongoDB
  - springboot
---
## 在项目中添加依赖
可以在建立项目时选定要使用的工具，也可以后期直接在 pom 文件中添加 dependency。
![[CleanShot 2024-07-08 at 09.09.22@2x.png|600]]
## 配置连接字符串
在 application 配置文件中设定即可。
![[CleanShot 2024-07-08 at 09.10.02@2x.png|550]]

## 启动数据库服务
在项目运行前，需要启动好数据库服务才行。直接使用 mongod 命令启动数据库服务，并配置 `–dbpath` 来指定数据库位置（已有则直接打开，没有创建新数据库开始）:
```shell
mongod --dbpath C:\Users\A\Desktop\hello-springDB\mongoData
```

# 具体的设计模式应用
---
## 分层设计管理的思想
- `Document` ：数据实体层，定义数据实体的内容
- `DAO`：数据（表）访问层，负责将 java 中的数据实体与数据库中的表映射，实现数据的 CRUD，是数据存取**接口**
- `Service`：服务层，通过使用DAO中封装好的CRUD来编写代码逻辑，提供所需要的服务

### 1. 设计数据实体（Document层）
用 @Document 来标识数据实体类，用 @id 来标识实体中的主键
![[CleanShot 2024-07-08 at 09.33.45@2x.png|500]]
### 2. 设计 Repository（DAO层）
需要继承 MongoRepository 接口，其中包含一些基本的数据 CRUD 操作。可以在其中根据需求，扩展新的方法...

记得加上 @Repository 注释!
![[CleanShot 2024-07-08 at 09.34.14@2x.png|500]]
### 3. 服务层访问数据库（Service层）

在 service 层的类中，直接注入数据实体的 repository 对象即可。通过 repository 对象可以直接调用封装好的数据操作功能。
![[CleanShot 2024-07-08 at 09.34.34@2x.png|500]]