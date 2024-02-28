```java
// 启动

Java -jar arthas-boot.jar

// 选择监控进程

[INFO] JAVA_HOME: /Users/nihaoya/Library/Java/JavaVirtualMachines/corretto-17.0.7/Contents/Home
[INFO] arthas-boot version: 3.7.2
[INFO] Found existing java process, please choose one and input the serial number of the process, eg : 1. Then hit ENTER.
* [1]: 800
  [2]: 43768 org.jetbrains.jps.cmdline.Launcher
  [3]: 43769 Test
  [4]: 42763
3

[INFO] arthas-client connect 127.0.0.1 3658
  ,---.  ,------. ,--------.,--.  ,--.  ,---.   ,---.
 /  O  \ |  .--. ''--.  .--'|  '--'  | /  O  \ '   .-'
|  .-.  ||  '--'.'   |  |   |  .--.  ||  .-.  |`.  `-.
|  | |  ||  |\  \    |  |   |  |  |  ||  | |  |.-'    |
`--' `--'`--' '--'   `--'   `--'  `--'`--' `--'`-----'


//查看类加载器
[arthas@43769]$ classloader -l
 name                                                loadedCount  hash      parent
 BootstrapClassLoader                                2695         null      null
 com.taobao.arthas.agent.ArthasClassloader@7172742c  1367         7172742c  sun.misc.Launcher$ExtClassLoader@2ac4ba13
 sun.misc.Launcher$AppClassLoader@18b4aac2           8            18b4aac2  sun.misc.Launcher$ExtClassLoader@2ac4ba13
 sun.misc.Launcher$ExtClassLoader@2ac4ba13           58           2ac4ba13  null
Affect(row-cnt:4) cost in 40 ms.


// 根据类加载器的hash码来查看具体加载的类
// 查看扩展类加载器的
[arthas@43769]$ classloader -c 2ac4ba13
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/sunec.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/nashorn.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/cldrdata.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/jfxrt.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/dnsns.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/localedata.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/sunjce_provider.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/sunpkcs11.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/jaccess.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/zipfs.jar
Affect(row-cnt:20) cost in 9 ms.


// 查看应用程序加载器的
[arthas@43769]$ classloader -c 18b4aac2
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/charsets.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/deploy.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/cldrdata.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/dnsns.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/jaccess.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/jfxrt.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/localedata.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/nashorn.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/sunec.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/sunjce_provider.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/sunpkcs11.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/ext/zipfs.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/javaws.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/jce.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/jfr.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/jfxswt.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/jsse.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/management-agent.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/plugin.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/resources.jar
file:/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home/jre/lib/rt.jar
file:/Users/nihaoya/Coding/IdeaCode/JavaSE/JustTest/out/production/JustTest/
file:/Applications/IntelliJ%20IDEA.app/Contents/lib/idea_rt.jar
file:/Users/nihaoya/.arthas/lib/3.7.2/arthas/arthas-agent.jar
Affect(row-cnt:48) cost in 8 ms.
```