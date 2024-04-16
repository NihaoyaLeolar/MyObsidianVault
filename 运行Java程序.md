---
tags:
  - java
---
# ⬛ 用命令行编译运行 
---
1. 使用`javac`命令编译：源文件 -> .class字节码文件
2. 使用java虚拟机中的java解释器（java.exe）来解释，执行其字节码文件(.class文件)
![[image 6.png]]
## 注意
- 主类必须和文件名相同
- javac编译的是一个.java文件，带后缀
- java解释器执行主类，不带后缀

# 📦 打包成jar运行 
---
这里以我的springboot项目为例，可以在pom文件里配置jar包的名字：
![[image 7.png|450]]
使用maven工具打包成jar运行:
![[image 8.png|400]]
打包完jar包会出现在target文件夹下：
![[image 9.png|300]]

直接使用命令行运行:

```shell
java -jar xxx.jar
```