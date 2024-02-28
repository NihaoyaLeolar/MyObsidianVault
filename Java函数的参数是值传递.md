>当传递一个对象到方法时，你实际上传递的是**对象引用的副本**，而不是对象本身。

- 不能改变原引用本身（不能使原引用指向一个新的对象）
- 但可以改变引用所指向的对象的状态。
## 例子
`modify` 方法接收了一个 `MyClass` 类型的对象引用的副本。在方法内，这个副本引用被用来修改对象的 `value` 字段。因为这个副本引用和原始引用都指向同一个对象，所以当在方法内部修改对象的状态时，这个改变会反映在原始对象上。
```java
public class Example {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        obj.value = 10;
        
        System.out.println("Before: " + obj.value); // 显示 10
        modify(obj);
        System.out.println("After: " + obj.value);  // 显示 20
    }
    public static void modify(MyClass obj) {
        obj.value = 20; // 修改了obj引用的对象的状态
    }
}

class MyClass {
    int value;
}
```
如果在方法内部尝试改变引用本身，比如说让它指向一个新的对象，这个改变不会影响到传入方法的原始引用：
```java
public static void modify(MyClass obj) {
    obj = new MyClass(); // 这里改变了引用，但只是副本引用
    obj.value = 30;
}
```

原始的 `obj` 引用仍然指向原始对象，而不是新创建的对象。
所以，**虽然Java使用的是值传递，但由于对象引用的特性，它在行为上更像是引用传递。**