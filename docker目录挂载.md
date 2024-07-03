---
tags:
  - 工具
---

1. 可以实现宿主机和容器、容器和容器之间共享文件
2. 实现docker容器持久化
# 挂载方式
---
![[Pasted image 20240607202427.png|500]]
## 1. Bind Mount
将宿主机下的某路径挂载到容器下的某路径
这种挂载方式是直接将宿主机上的文件或目录与容器中的路径进行关联，容器中的应用程序可以直接访问和修改挂载的文件或目录。
```shell
docker run -v <host_dir>:<container_dir> <image_name> 
```

## 2. Volume Mount
由容器在宿主机上创建一个卷，然后挂载到容器某路径下
>卷(Volume)是 Docker 管理的存储区域，依旧是在宿主机上。。容器中的应用程序可以访问和修改挂载的卷，而卷中的数据会在容器之间共享。

```shell
docker run -v <volume_name>:<container_dir> <image_name>
```

## 3. tmpfs Mount
适合存储临时文件，存宿主机内存中。不可多容器共享。