---
tags:
  - java
---
`Comparable` 和 `Comparator` 都是 Java 中用于比较对象的机制，但它们有一些区别。
# 都是用于定义比较顺序
## Comparable 接口
>如果一个类实现了 Comparable 接口，就意味着这个类的对象本身**具有内在的比较能力**。
- 方法：需要实现 `compareTo` 方法，该方法接收一个同类型的对象作为参数，并返回一个整数值，表示当前对象与参数对象的比较结果。
- 用途：常用于**定义类的自然排序顺序**。例如，String 和 Integer 等类都实现了 Comparable 接口，具有默认的自然排序规则。
- **唯一性**：一个类只能实现一次 Comparable 接口，因此只能有一种默认的排序方式。

eg：
```java
class Person implements Comparable<Person> {
    private String name;
    private int age;
	// 省略 getter setter constructor
    
    @Override  // 实现 Comparable 接口的 compareTo 方法，按照年龄升序排序
    public int compareTo(Person other) {
        return this.age - other.age;
    }
}
```
## Comparator 接口
>是一个**独立于类的外部**比较器，可以用于**定义不同的比较策略**。
- 方法：需要实现 `compare` 方法，其参数和返回值与 Comparable 的 compareTo 方法类似。
- 用途：当需要为一个类提供多种不同的比较方式，或者一个类本身没有实现 Comparable 接口但又需要进行排序操作时，就可以使用 Comparator 。
- **灵活性**：可以创建多个不同的 Comparator 对象来实现不同的排序逻辑，更加灵活。

eg：
```java
class PersonNameLengthComparator implements Comparator<Person> {
    @Override
    public int compare(Person p1, Person p2) {
        return p2.getName().length() - p1.getName().length();
    }
}
```
# 两者区别
---
总之，`Comparable` 是内置于类中的比较逻辑，而 `Comparator` 是独立于类的外部比较器，可以根据具体需求在不同场景下灵活选择使用。

- 如果希望为一个类定义其默认的、自然的排序方式，并且这种排序方式是内在且固定的，与类的本质特性紧密相关，那么应该让这个类实现 `Comparable` 接口。
	- 例如，对于表示学生的 Student 类，如果默认按照年龄来排序是合理且固定的，那么让 Student 类实现 Comparable 接口并定义相应的比较逻辑是合适的。
- 如果需要根据不同的场景或条件为类提供多种不同的排序方式，或者类本身已经无法修改（例如是第三方库中的类）但又需要进行排序操作，那么应该使用 `Comparator` 。
	- 例如，对于 Student 类，有时需要按照年龄排序，有时需要按照成绩排序，就可以创建不同的 Comparator 来实现这些不同的排序策略。

eg:
```java
public class PersonExample {
    public static void main(String[] args) {
        // 使用 Comparable 进行排序（按照年龄）
        TreeSet<Person> ageSortedSet = new TreeSet<>();
        ageSortedSet.add(new Person("Alice", 25));
        ageSortedSet.add(new Person("Bob", 30));
        ageSortedSet.add(new Person("Charlie", 20));
        System.out.println("按照年龄排序: " + ageSortedSet);

        // 使用 Comparator 进行排序（按照名字长度）
        TreeSet<Person> nameLengthSortedSet = new TreeSet<>(
	        new PersonNameLengthComparator()
	    );
        nameLengthSortedSet.add(new Person("David", 28));
        nameLengthSortedSet.add(new Person("Emma", 22));
        nameLengthSortedSet.add(new Person("Frank", 35));

        System.out.println("按照名字长度排序: " + nameLengthSortedSet);
    }
}
```


# Java中现成的比较器
---
Java 的`java.util`包中提供了一些现成的比较器，例如：
1. `Comparator.reverseOrder()`: 返回一个比较器，按照自然顺序的逆序进行比较。如果是数值类型，就是从大到小排序；如果是字符串，就是按照字典序的逆序排序。
2. `Comparator.naturalOrder()`: 返回一个比较器，按照元素的自然顺序进行比较。

注意，这里的自然顺序，是[[自带排序逻辑的类]]的排序结果（实现了comparable接口，重写 compareTo 定义的）

eg：
```java
public class BuiltInComparatorExample {
    public static void main(String[] args) {
        // 使用 naturalOrder 按照整数升序排序
        TreeSet<Integer> ascendingSet = new TreeSet<>(Comparator.naturalOrder());
        ascendingSet.add(5);
        ascendingSet.add(2);
        ascendingSet.add(8);
        System.out.println("升序排序: " + ascendingSet);
        
        // 使用 reverseOrder 按照整数降序排序
        TreeSet<Integer> descendingSet = new TreeSet<>(Comparator.reverseOrder());
        descendingSet.add(5);
        descendingSet.add(2);
        descendingSet.add(8);
        System.out.println("降序排序: " + descendingSet);

        
    }
}
```