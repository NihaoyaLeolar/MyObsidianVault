>通过阅读和跟踪调试JDK源码，从而了解Java技术体系的原理！

实在是跟着书本看不懂，最后跟着程序羊[JDK手动编译](https://www.bilibili.com/video/BV1zT4y177Zf/?spm_id_from=333.337.search-card.all.click&vd_source=2801815325491e9a2af89f7b23e58173)
![[JDK都没手动编译过，敢说自己是Java程序员吗？实战编译Java源码（JDK源码,JVM）视频教程 - 001 - JDK都没手动编译过，敢说自己是Java程序员吗？实战编译Java源码（JD….mp4]]

🔑 **重要**：
最终我在M1芯片的MacOS13上，成功使用JDK17编译成功了JDK18！
感谢[这篇文档](https://segmentfault.com/a/1190000041074433)救我小命，可以按时下班了！！！
# 1. 获取JDK源码
---
## 获取方式
1. 通过Mercurial代码版本管理工具，从[Repository](http://hg.openjdk.java.net/jdk7u/jdk7u)直接获取源码！但是说是速度有点慢，而且我也不太会用！
2. 直接下载[官方](http://jdk7.java.net/source.html)打包好的源码包，但是这个网址已经失效
3. 其他办法：按照[这里](https://www.jianshu.com/p/6fe47f6a1b2a)的方式下载，或者[点击这个](https://jdk.java.net/)找到对应版本下载

# 2. 安装所需环境
---
## Linux 虚拟机下
- Bootstrap JDK: 之前已经装好过，/usr/local/lib/jdk1.8
- gcc
- 以下这些
```shell
sudo apt-get install build-essential gawk m4 libasound2-dev libcups2-dev libxrender-dev xorg-dev xutils-dev x11proto-print-dev binutils libmotif-dev ant
```

只有`x11proto-print-dev`安装出现了点问题，其他的都很顺利：
```text
Unable to locate package x11proto-print-dev
```

很奇怪，暂且放弃在linux

## 在MacOS下
基本上有了下面的就可以，其他的等到configure有报错再去管。毕竟很迷...

```shell
Java -version
# java version "1.8.0_281"

xcode 打开看版本
# 14.3.1

clang++ --version
# Apple clang version 14.0.3 (clang-1403.0.22.14.1)
clang --version
# Apple clang version 14.0.3 (clang-1403.0.22.14.1)

autoconf --version
# 使用brew新安装：autoconf (GNU Autoconf) 2.71

make --version
# 3.81

freetype-config --ftversion
# 使用brew新安装：2.13.1
```

# 3. Configure
----
下载完的源码，解压后重命名，文件结构为：
![[CleanShot 2023-08-03 at 16.21.33.png|480]]
在源码根目录下，执行:
```shell
sh configure
```
即可开始配置。配置完成后会打印。完成的配置文件在：
![[CleanShot 2023-08-03 at 16.53.04.png]]
大概率因为各种版本问题，期间会出现很多错误。遇到这些问题，我参考大家的解决方式大多都是：
1. 环境不通过的，安装环境
2. 环境版本不通过的，重新安装其他版本
3. 其他错误，直接更改configure脚本，将相关代码注释掉直接通过...

>所以，我觉得大多数是因为版本和适配性问题，不是什么技术问题，没有必要钻！我的建议是直接换更高的jdk版本来弄，比如我卡在jdk8上很久，换jdk18就很快配置通过。

因为我还是钻了一些问题，还是记录一下：
#### 问题1. gcc/g++的默认版本问题
不知道为什么，苹果自带的GCC和G++无法通过，因为默认使用的Clang...
所以只好重新下载啦：
```shell
# gcc/g++两个需要通过brew下载新的，通过ln链接到/usr/local/bin下
sudo ln -s /opt/homebrew/bin/gcc-13 /usr/local/bin/gcc
sudo ln -s /opt/homebrew/bin/g++-13 /usr/local/bin/g++
```
#### 问题2. target 位数问题
```text
error: The tested number of bits in the target (0) differs from the number of bits expected to be found in the target (32)
```

修改配置文件：打开`common/autoconf/generated-configure.sh`，搜索这个语句，把判断语句用到的两个变量改为32即可。如：
```shell
TESTED_TARGET_CPU_BITS=`expr 8 \* $ac_cv_sizeof_int_p`
# 加上这一句(视情况而定，我这边改成32位)
TESTED_TARGET_CPU_BITS=32

   if test "x$TESTED_TARGET_CPU_BITS" != "x$OPENJDK_TARGET_CPU_BITS"; 
......
```


# 4. 编译
----
在源码的根目录下，输入如下指令，可以根据刚才配置好的开始编译：
```shell
# 全量编译
make all
```

太开心了，成功了！编译完输出这样：
![[CleanShot 2023-08-03 at 18.11.29.png]]

#### 问题1: make出现源码语法错误
如果在编译时，出现编译`sprintf()`和`__deprecated__(_msg)`的Error提示。使用下面的命令重新configure，可以解决！神奇地不再Error，而是疯狂Wrang！
```shell
bash ./configure --with-debug-level=slowdebug --with-jvm-variants=server --disable-warnings-as-errors --with-extra-cflags=-stdlib=libc++ --with-extra-cxxflags=-stdlib=libc++ --with-extra-ldflags=-stdlib=libc++
```

# 5. 成果说明
---
源码目录下：
- 编译产生的东西：`build/macosx-aarch64-server-slowdebug`
- 编译生成的JDK：`build/macosx-aarch64-server-slowdebug/images/jdk`，这个和我们平时直接下载使用的JDK性质上没什么区别！

查看编译出来的java命令，会有我的用户名在里头：
![[CleanShot 2023-08-03 at 18.59.50@2x.png]]
## 在IDEA中使用
`cmd + ;`打开 Project Structure：
![[CleanShot 2023-08-03 at 19.36.02@2x.png]]
这样跑起来就是用自己的JDK编译运行的了
但是出现了一个问题：编译后运行很慢...
## 在IDEA中阅读+更改
阅读很简单，如果需要更改则需要在`SourcePath`下，把源码改成`源码路径下的src的所有文件`：
![[CleanShot 2023-08-03 at 19.42.14@2x.png]]
更改完后需要重新到源码文件夹下执行如下命令，快速地**增量编译**：
```xml
make images
```

这样更改JDK源码就会生效了！！！