---
tags:
  - 工具
---
# 🐬 常用命令
---
```shell
# 查看运行
docker ps

# 查看mongo服务的端口映射
docker port mongo

# 进入容器
docker attach 容器ID
docker exec -it 容器ID /bin/bash 
docker exec -it 容器Name bash

#退出容器
exit
```