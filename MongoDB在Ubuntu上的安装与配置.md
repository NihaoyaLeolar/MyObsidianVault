---
tags:
  - MongoDB
  - 环境配置
---
# 🍃 Ubuntu 快速安装 mongoDB
---
直接安装：`sudo apt-get install mongodb` ，安装后应该已经默认运行了。
查看运行情况和路径：`ps -ef | grep mongo`
## 运行报错解决
跑起来报错 SocketException: Address already in use
```shell
sudo lsof -iTCP -sTCP:LISTEN -n -P
sudo kill <mongo_command_pid>
```

# 🔧 基本配置
---
设置外网可以访问，默认到配置文件里设置只有本机可以访问到服务
![[image 2.png]]

# ⏸️ 启停
---
```shell
sudo systemctl start mongod
sudo systemctl status mongod
sudo systemctl stop mongod
sudo systemctl restart mongod
```