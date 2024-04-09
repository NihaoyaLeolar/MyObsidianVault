---
tags:
  - mysql
  - 缓存
---
![[CleanShot 2024-02-05 at 13.50.07@2x.png]]
![[CleanShot 2024-02-05 at 14.25.36@2x.png]]
## SQL - Structured Relational
1. **结构化**
- 存入的数据都有类型要求
- 一般在设计时就定好表结构，开发时尽量不改变
![[CleanShot 2024-02-05 at 13.54.08@2x.png|350]]
2. **关联的**
- 表与表之间的关系使用外键定义
![[CleanShot 2024-02-05 at 14.13.46@2x.png|360]]
3. **SQL查询**
- 语法固定查询，都是使用SQL
```sql
SQL SELECT id, name age FROM tb_user WHERE id = 1
```

4. **ACID**
满足事物的ACID特性
## NoSQL - No / Not Only
1.  **非结构化**
- 存储的数据可以是键值型(redis)、文档型(MongoDB)、列类型(HBase)、图类型(Neo4j)：
![[CleanShot 2024-02-05 at 14.08.08@2x.png|400]]
- 对数据的结构没有严格要求，key/value的数据类型都可以自定义

2. **非关联的**
- 通过json文档嵌套来描述相关信息（缺点是会出现重复）
- 数据库本身不会维护关联关系，需要程序员本身通过业务逻辑来维护
![[CleanShot 2024-02-05 at 14.15.22@2x.png|360]]
3. **非SQL**
- 没有固定的语言和语法，不同数据库有设计不同的命令/请求/函数
![[CleanShot 2024-02-05 at 14.18.40@2x.png|500]]
4. **BASE**
只能保持基本的（甚至没有）一致性