---
tags:
  - 环境配置
  - java
---
tips：获取安装包，需要在oracle官网下载，指定小版本的很难找，所以直接在浏览器搜索：如jdk 1.8 202，就会有链接～
# ☕️ 在 Linux 上安装
---
安装 oracle jdk 基本上都要手动安装。**安装步骤：**
1. 直下载压缩包，为 .tar.z 格式
2. 然后解压缩，将包放到`usr/local/java`下，现在就会有一个文件夹，`usr/local/java/jdk1.8.0_202`
3. 把`usr/local/java/jdk1.8.0_202/bin`写到环境变量里

# ☕️ 在 MAC 上安装
---
## 安装位置
本mac下已经安装了的 JDK 在这`/Library/Java/JavaVirtualMachines`：
![[image 4.png]]

## 安装步骤

1. 下载安装包，为.dmg格式
![[image 3.png|160]]
2. 打开dmg直接进入安装页面，一直点到完成，JDK就会出现在`/Library/Java/JavaVirtualMachines`中

## 使用方式
- 写入环境变量然后可以直接在命令行使用～
- 使用 IDE 如 IntelliJ，可以直接找到指定JDK开发
![[image 5.png]]