---
tags:
  - 工具
---
[一小时速通教程](https://docker.easydoc.net/doc/81170005/cCewZWoN/lTKfePfP)
[官方文档-命令]([https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/))
# 基本概念
----
**镜像**：可以理解为软件安装包，可以方便的进行传播和安装。  
**容器**：软件安装后的状态，每个软件运行环境都是独立的、隔离的，称之为容器。

# 上手使用
----
- 用用就清楚了，试试[[使用docker安装redis环境]]
- 当宿主机和容器or容器之间需要共享文件时，这就需要了解到[[docker目录挂载]]了

# 🐬 常用命令
---
```shell
# 调配
docker pull    # 用于从 Docker 镜像仓库中拉取镜像
docker build   # 根据指定的 Dockerfile 构建一个新的 Docker 镜像
docker images  # 用于列出本地所有的 Docker 镜像
docker rm      # 用于删除一个已经停止的 Docker 容器

# 查看运行信息
docker ps                    # 列出当前正在运行的 Docker 容器
docker port <service_name>   # 查看服务的端口映射

```

```shell
# 启停
docker run <image_name>    # 根据镜像创建并运行一个新的 Docker 容器
docker-compose up          # 启动 Docker Compose 应用程序（根据指定的 `docker-compose.yml` 文件中的配置信息，创建并启动应用程序所需的服务）
docker stop                # 停止一个正在运行的 Docker 容器
```


`docker-compose` 是一个用于定义和运行多容器 Docker 应用程序的工具。它通过一个 `docker-compose.yml` 文件来描述应用程序中各个容器的配置、依赖关系和网络设置等信息。然后，使用 `docker-compose up` 命令来启动整个应用程序，它会按照配置文件中的定义创建和启动所有相关的容器，并自动处理容器之间的连接和依赖关系。

相比之下，使用 `docker run` 命令需要手动为每个容器指定各种参数，如镜像、端口映射、环境变量等。当涉及到多个容器时，这会变得非常繁琐和容易出错，并且难以管理容器之间的关系。

```shell
# 进入容器
docker attach 容器ID
docker exec -it 容器ID /bin/bash 
docker exec -it 容器Name bash

# 退出容器
exit
```

```shell
# 关于卷
docker volume ls                    # 查看docker中所有的卷
docker volume inspect <volume_name> # 查看卷的信息
```