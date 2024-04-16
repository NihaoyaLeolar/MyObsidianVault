---
tags:
  - Python
---
>python装各种环境其实就是装各种包，pip是一个很好的包管理工具

# python如何找包
----
[【CSND】python找包解析](https://blog.csdn.net/qq_19645269/article/details/104590587?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163691206916780261928466%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=163691206916780261928466&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-104590587.pc_search_result_cache&utm_term=Python%E6%98%AF%E5%A6%82%E4%BD%95%E6%89%BE%E5%8C%85%E7%9A%84%2CPython%E5%AE%89%E8%A3%85%E7%9A%84%E5%8C%85%E6%94%BE%E5%9C%A8%E5%93%AA%E9%87%8C.&spm=1018.2226.3001.4187)

看看 `sys.path`:

![](https://cdn.nlark.com/yuque/0/2021/png/25369650/1636971444590-af722e85-6985-4636-8747-d5784e0d79e9.png)
会在以上这些路径下找包！
可以知道，python2/3的依赖包肯定是不在一个路径下的

# Python2/3 在配置上的区别
---

输入python，会显示python2.x

输入 python3，会显示python3.x

那么默认python命令是默认python2，python3命令默认python3

同理 pip/pip3 命令也是，分别管理python2/python3的包