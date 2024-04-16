---
tags:
  - 工具
  - 多线程编程
---
# 添加 Maven 依赖
---
工具在openjdk提供的jol包下
```xml
<dependency>  
    <groupId>org.openjdk.jol</groupId>  
    <artifactId>jol-core</artifactId>  
    <version>0.15</version>  
</dependency>
```

# 使用
---
```java
import org.openjdk.jol.info.ClassLayout;  
  
@Slf4j  
public class TestBiased {  
    public static void main(String[] args) {  
        Dog dog = new Dog();  
        log.debug(ClassLayout.parseInstance(dog).toPrintable());  
    }  
}  
  
class Dog {  
  
}
```
打印结果如下，可以看到对象头中的 Mark Word =  0x0000000000000001
```text
16:05:20.495 [main] DEBUG org.example.TestBiased - org.example.Dog object internals:
OFF  SZ   TYPE DESCRIPTION               VALUE
  0   8        (object header: mark)     0x0000000000000001 (non-biasable; age: 0)
  8   4        (object header: class)    0xf800ef95
 12   4        (object alignment gap)    
Instance size: 16 bytes
Space losses: 0 bytes internal + 4 bytes external = 4 bytes total
```
