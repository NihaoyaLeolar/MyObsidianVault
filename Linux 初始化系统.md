> 初始化系统是操作系统的一部分，负责在系统引导时启动各个系统组件和服务。
> 在 Linux 系统中，初始化系统负责整个系统的启动过程。其主要任务包括**启动内核、配置硬件、启动用户空间进程**以及**管理系统服务**。

# 初始化系统的两种类型
----
在现代的 Linux 系统中，大多数发行版都采用了 `systemd` 作为默认的初始化系统。systemd 提供了一种更为先进和全面的方式来管理系统进程和服务。然而，一些旧的系统或特殊用途系统可能仍然使用 `SysVinit` 或其他初始化系统。

1. **systemd：**
    - systemd 是一个现代的初始化系统，设计用于替代传统的 SysVinit 和其他一些初始化系统。
    - 大多数主流的 Linux 发行版，如 Ubuntu、Fedora、Debian 等，都采用了 systemd 作为默认的初始化系统。
    - 使用 `systemctl` 命令进行服务管理。
2. **SysVinit：**
    
    - SysVinit 是一个传统的初始化系统，已经存在很长时间，并在许多旧的 Linux 发行版中使用。
    - 通过使用 `service` 命令或直接调用启动脚本来管理服务。
    - 逐渐被 systemd 取代，但仍然可以在一些系统中找到。

有一些其他的初始化系统和变种，但 systemd 和 SysVinit 是两个主要的选择。如果你的系统不是 systemd，很可能是 SysVinit 或其他一些自定义的方案。大多数新的 Linux 发行版都转向了 systemd，因为它提供了更多的功能和性能优势。

# 如何知道本机器使用的是哪一种初始化系统
---

### 1. 查看进程
使用 `ps` 命令查看 PID 为 1 的进程，通常这是初始化系统的进程。
```shell
ps -p 1 -o comm=
```
结果中会显示正在运行的初始化系统的名称，例如 `systemd` 或 `init`。
### 2. 查看 /proc 文件系统
查看 `/proc` 文件系统中的 `/proc/1/exe` 符号链接，它指向 PID 为 1 的进程的可执行文件。
```shell
ls -l /proc/1/exe
```
### 3. 查看 /etc 目录
检查 `/etc` 目录下的初始化系统相关的文件，例如 `/etc/systemd/` 或 `/etc/init.d/`
```shell
ls /etc
```
这里可能会找到 systemd 相关的目录或 SysVinit 脚本

# 管理服务的命令使用
---
目前使用服务器开发，使用频率较多的就是对服务的管理，下面记录一些常用的命令
```shell
sudo systemctl start <service_name>     # 启动服务
sudo systemctl stop <service_name>      # 停止服务
sudo systemctl restart <service_name>   # 重启服务
sudo systemctl status <service_name>    # 查看服务状态

sudo systemctl enable <service_name>     # 启用开机自启
sudo systemctl disable <service_name>    # 禁用开机自启
sudo systemctl is-enabled <service_name> # 检查是否开机自启

journalctl -u <service_name>             # 查看服务的日志
```

# Systemd 管理服务的原理
----
### systemd 单元文件
> systemd 使用单元(unit)文件来描述系统中的各个组件(服务、套接字、挂载点等)
> 对于服务，单元文件通常是以`.service`结尾的，如`mysql.service`，用于定义和配置一个 systemd 服务。

**结构：**
```shell
+------------------+
|   [Unit]         |
|                  |
|   Description=   |
|   After=         |
|   Wants=         |
+------------------+
|   [Service]      |
|                  |
|   Type=          |
|   ExecStart=      |
|   Restart=       |
|   ...            |
+------------------+
|   [Install]      |
|                  |
|   WantedBy=      |
+------------------+
```
1. **`[Unit]` 部分：**
    - `Description`: 描述服务的简短说明，通常是对服务的一个简洁描述。
    - `After`: 指定服务应该在哪些其他单元之后启动。
    - `Wants`: 指定服务需要的其他单元。
2. **`[Service]` 部分：**
    - `Type`: 指定服务的类型，如 `simple`、`forking`、`oneshot` 等。
    - `ExecStart`: 指定启动服务时要执行的命令或脚本。
    - `Restart`: 指定服务在发生故障时是否重新启动。
3. **`[Install]` 部分：**
    - `WantedBy`: 指定服务被启用（enabled）时，将链接到哪个目标（target）。

我机器的单元文件在/lib/systemd/system/下，都是以.service结尾的文件。

通过sudo systemctl status的输出，可以看服务对应的单元文件的路径在哪里，也就知道本系统的单元文件路径位置了。一般在安装时，就已经把对应服务的单元文件形成放在管理目录下。
```shell
sudo systemctl cat <service_name>        # 查看服务的单元文件，可以看到服务的名称、启动命令等
```

### 原理
**Systemd通过服务名，查询到对应服务的单元文件，然后执行文件中对应路径的命令，从而完成执行。**

比如以服务启停来解读管理流程：
- 当使用 `systemctl start` 启动一个服务时，systemd 会读取对应的服务单元文件（例如 `/etc/systemd/system/mysql.service`），并执行该单元文件中指定的 `ExecStart` 命令，启动相应的服务进程。
- 当使用 `systemctl stop` 或 `systemctl restart` 停止或重启服务时，systemd 会执行单元文件中的 `ExecStop` 命令，关闭或重新启动服务进程。
