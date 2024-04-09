---
tags:
  - jvm
---
[[Java 字节码存在项目 out 文件夹下]]，由于java字节码是二进制的，查看java字节码需要使用工具。可以使用原生javap指令，也可以使用一些插件工具查看。
![[CleanShot 2023-12-25 at 18.43.34@2x.png|600]]
# 使用javap命令
----
`javap`是jdk自带的反编译工具，可以查看java字节码。一般用于直接在服务器端上黑窗操作查看。
```shell
~/Coding/IdeaCode/JavaSE/JustTest/out/production/JustTest  javap -v Test.class
```
Test.java源码：
```java
public class Test {  
    public static void main(String[] args) {  
        int i = 100;  
        int j = 200;  
        int x = i + j;  
    }  
}
```

字节码：
```
 Last modified 2023年12月22日; size 424 bytes
  SHA-256 checksum ca061c57b6acb6f1445411fecbe691d9e31f2bd6aaa49f443cb95b02864972c7
  Compiled from "Test.java"
public class Test
  minor version: 0
  major version: 52
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #2                          // Test
  super_class: #3                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 2, attributes: 1
Constant pool:
   #1 = Methodref          #3.#21         // java/lang/Object."<init>":()V
   #2 = Class              #22            // Test
   #3 = Class              #23            // java/lang/Object
   #4 = Utf8               <init>
   #5 = Utf8               ()V
   #6 = Utf8               Code
   #7 = Utf8               LineNumberTable
   #8 = Utf8               LocalVariableTable
   #9 = Utf8               this
  #10 = Utf8               LTest;
  #11 = Utf8               main
  #12 = Utf8               ([Ljava/lang/String;)V
  #13 = Utf8               args
  #14 = Utf8               [Ljava/lang/String;
  #15 = Utf8               i
  #16 = Utf8               I
  #17 = Utf8               j
  #18 = Utf8               x
  #19 = Utf8               SourceFile
  #20 = Utf8               Test.java
  #21 = NameAndType        #4:#5          // "<init>":()V
  #22 = Utf8               Test
  #23 = Utf8               java/lang/Object
{
  public Test();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 4: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   LTest;

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=4, args_size=1
         0: bipush        100
         2: istore_1
         3: sipush        200
         6: istore_2
         7: iload_1
         8: iload_2
         9: iadd
        10: istore_3
        11: return
      LineNumberTable:
        line 7: 0
        line 8: 3
        line 9: 7
        line 10: 11
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      12     0  args   [Ljava/lang/String;
            3       9     1     i   I
            7       5     2     j   I
           11       1     3     x   I
}
SourceFile: "Test.java"

```

# 使用 jclasslib 插件
idea中有插件，直接安装即可
![[CleanShot 2023-12-25 at 18.54.49@2x.png]]
![[CleanShot 2023-12-25 at 18.53.55@2x.png]]