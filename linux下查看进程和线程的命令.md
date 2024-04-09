---
tags:
  - 多线程编程
---
# 基本的
---
查看所有进程，带查找筛选：
```shell
ps ｜ grep <string>
```

使用jdk自带的命令行工具查看java进程
```shell
jps
```

杀死进程
```shell
kill <pid>
```

# 复杂一点的
---
查看详细的进程信息，会实时更新显示资源占比
```shell
top  
```

查看线程信息，需要输入进程的id
```shell
top <pid>
jstack <pid>  //这是jdk自带的
```
