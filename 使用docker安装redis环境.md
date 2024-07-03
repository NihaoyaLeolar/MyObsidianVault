---
tags:
  - 工具
---
# 一行命令直接运行
---
```shell
docker run -d -p 6379:6379 --name redis redis:latest
```
- `docker run`：启动一个新的 Docker 容器。
- `-d`：以守护进程（daemon）模式运行容器，即在后台运行。
- `-p 6379:6379`：将容器的 6379 端口映射到主机的 6379 端口，以便可以从主机访问 Redis 服务。
- `--name redis`：为容器指定一个名称为`redis`。
- `redis:latest`：指定要使用的 Redis 镜像，`latest`表示使用最新版本的 Redis 镜像。本地没有找到镜像就会到仓库拉取匹配的镜像
```shell
nihaoya@LittleCats-MBP  ~  docker run -d -p 6379:6379 --name redis redis:latest
Unable to find image 'redis:latest' locally
latest: Pulling from library/redis
24c63b8dcb66: Pull complete
3fe6ac530baf: Pull complete
4c8c5b88360b: Pull complete
a658802c95ef: Pull complete
1ae9a6609c71: Pull complete
fedba7f39f61: Pull complete
4f4fb700ef54: Pull complete
d3590ea3f08d: Pull complete
Digest: sha256:01afb31d6d633451d84475ff3eb95f8c48bf0ee59ec9c948b161adb4da882053
Status: Downloaded newer image for redis:latest
bc094a5b09ecadc72132168c1c215aabe8c76944dbd33ae3d8f7eb8883d3cde1
```

# 访问命令行
---
直接进入镜像对应的页面，可以获得镜像虚拟机的命令行环境
![[CleanShot 2024-06-07 at 19.12.56@2x.png]]
