---
tags:
  - MongoDB
  - 环境配置
---
在mac上安装的记录
# 🌲 MongoDB安装与环境配置
---
1. 在官网下载包

![[CleanShot 2024-04-10 at 17.41.22@2x.png]]

2. 下载后解压，然后将解压后的包放在/usr/local中

![](https://cdn.nlark.com/yuque/0/2022/png/25369650/1651236992124-53eb10cc-bac0-4e27-a219-d7d1e1da3792.png)

3. 添加环境变量，在～./bash_profile中添加：

![](https://cdn.nlark.com/yuque/0/2022/png/25369650/1651236799164-c36e944b-6cf2-488f-9c66-baf42795320e.png)

4. 安装成果：

![](https://cdn.nlark.com/yuque/0/2022/png/25369650/1651237193508-40c44220-e440-4b4f-8c82-8597abe2de03.png)：
# ⭐️ 整个配置文件
----
创建文件夹 MongoData，并分别建立如下图所示文件夹：

![](https://cdn.nlark.com/yuque/0/2022/png/25369650/1651237867996-78c6a381-6742-448e-8acf-0c95aca71179.png)

- 在data文件夹中在创建 db文件夹
- 在etc文件夹中创建mongo.conf文件（注意后缀为.conf，为MongoDB的配置文件）
- 在log文件夹中创建mongo.log文件（用于放置MongoDB中的日志）
# 🤖 🍃基本使用
---

```shell
mongod -f /Users/nihaoya/DataBase/MongoData/etc/mongo.conf
```

- `mongod` 命令：启用数据库服务，即搭建并开启服务器，可以通过端口被访问.mongod是处理MongoDB系统的主要进程。它处理数据请求，管理数据存储，和执行后台管理操作。当我们运行mongod命令意味着正在启动MongoDB进程,并且在后台运行。
- `mongo` 命令 ：连接数据库服务，即连接服务器，可以通过端口进行访问.它是一个命令行工具用于连接一个特定的mongod实例。当我们没有带参数运行mongo命令它将使用默认的端口号和localhost连接

  



不知道为什么就是启动不上，各种奇怪的问题，准备用homebrew装试试～