
>它是一个结构严谨、面向对象的编程语言，最重要的是它**摆脱了硬件平台的束缚**，实现了“一次编写，到处运行”的理想。


# JDK 与 JRE
----
- JDK：Java程序开发最小环境 ———— Java程序设计语言、Java虚拟机、Java API 类库
- JRE：java程序运行最小环境 ———— Java虚拟机、Java API 类库子集
# JDK版本问题
----
- 开发版本号 —— Developer Version：程序员内部使用，如JDK1.5，如`java -version`的输出
- 公开版本号 —— Product Version：如JDK5

# Oracle JDK 与 Open JDK
----
>OpenJDK 是2006年，Sun公司将Java开源而形成的项目。
>源码开放、且可以被复用，有很多从OpenJDK上再次开发衍生出来的其他JDK发行版！

- **Oracle JDK** ：采用了商业实现，会存在少量OpenJDK没有的的商用闭源功能！
- **OpenJDK** ：使用的是开源的FreeType

对于JDK1.7而言，**两者共用了大量相同代码！OpenJDK基本上可以认为性能、功能和执行逻辑海上都和官方的Oracle JDK是一致的！**

