---
tags:
  - MongoDB
---

1. 创建账户
2. 创建新的 cluster
3. 在cluster上创建用户
4. 生成访问uri

在已经创建的 cluster 上找到 connect，提供compass的连接方式，可以看到访问uri
![[CleanShot 2024-07-08 at 09.06.47@2x.png|500]]
uri: nihaoya是我这个数据库的用户名，<password>处需要改成密码。粘贴到compass即可连接。同时在springboot的配置文件中，加入如下配置即可连接：

```
spring.data.mongodb.uri = mongodb+srv://nihaoya:abcpk123pk@cluster0.ysltv.mongodb.net/test:27017
```