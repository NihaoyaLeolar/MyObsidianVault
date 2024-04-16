---
tags:
  - Python
---
记录一些语法点，没有系统学过，零零散散的，容易忘
# ⛰️ 类
----
类里面有两个特别的：
- `self`：代表实例本身
- `cls`：代表类本身

### 变量

- 类变量，在类里显示定义，类的所有实例共有（像java的静态变量）
- 实例变量，各实例私有

### 方法

- 构造函数：命名为`__init__`。默认第一个需要传入self（可以取名为别的）
- 实例方法：通过实例调用。默认第一个需要传入self（可以取名为别的）
- 类方法：通过类名调用。默认第一个需要传入cls（可以取名为别的）
- 静态方法：通过类名调用。默认第一个需要传入cls（可以取名为别的）

### 示例

```python
class MyClass:
    # 类变量
    class_var = "This is a class variable."

    # 构造函数
    def __init__(self, instance_var):
        # 实例变量
        self.instance_var = instance_var

    # 实例方法
    def instance_method(self):
        print("This is an instance method.")
        print("instance_var =", self.instance_var)

    # 类方法
    @classmethod
    def class_method(cls):
        print("This is a class method.")
        print("class_var =", cls.class_var)

    # 静态方法
    @staticmethod
    def static_method():
        print("This is a static method.")
```

```python
# 访问类属性
print(MyClass.class_var)  # 输出: This is a class variable.

# 创建类的实例
obj = MyClass("This is an instance variable.")

# 通过实例调用实例方法
obj.instance_method()

# 通过类名调用类方法/静态方法
MyClass.class_method()
MyClass.static_method()
```

# 🛀 方法
---
## 下划线命名

在 Python 中，函数名称中的下划线有不同的含义，具体取决于下划线的位置和使用方式。

下面是常见的几种情况：

1. **单个前导下划线（_）：**内部使用 or 私有函数的约定。这意味着这些函数是供内部使用的，不应该被直接调用或访问。它们通常是在类的内部定义的，用于辅助其他公共方法或实现内部功能。
2. **单个尾随下划线（_）**：就是正常的方法，目的是为了避免与 Python 关键字或内置函数名产生冲突。虽然它们在语法上是有效的函数名称，但是在编程习惯中，它们通常用作标识符的后缀，以避免与其他名称冲突。
3. **双前导下划线（__）：**这种称为名称修饰符或名称修饰符，这样的函数名称在类中使用时会发生名称重整（name mangling）。在名称重整过程中，Python 解释器会将双下划线开头的函数名称转换为带有类名前缀的新名称，以防止子类中的名称冲突。这种机制用于实现类的封装和名称空间隔离。

```python
# __private_method 方法会在名称重整过程中转换为 _MyClass__private_method，
# 以确保在子类中不会与相同的方法名冲突。

class MyClass:
    def __private_method(self):
        pass
```

4. **双前导和双尾随下划线（__）**：特殊方法（special methods）或魔术方法（magic methods），用于在类中定义特定的行为或操作。这些方法具有特殊的功能，并由 Python 解释器自动调用。例如，**__init__** 方法用于初始化对象，在创建新实例时自动调用。

总之，Python 中的下划线在函数名称中具有不同的含义，可以用于指示内部使用、避免名称冲突、实现名称修饰符或定义特殊方法。这些约定和规则可以帮助开发人员编写更具表达力和易于理解的代码。

## 静态方法 V.S. 类方法

1. **参数传递：**

- 静态方法：静态方法没有特殊的参数传递要求，不会自动传递实例或类作为参数。在静态方法的定义中不需要包含 **self** 或 **cls** 参数。
- 类方法：类方法使用 **@classmethod** 装饰器进行标记，会自动传递类本身作为第一个参数，习惯上用 **cls** 表示。在类方法的定义中必须包含 **cls** 参数。

2. **访问属性和方法：**

- 静态方法：静态方法无法直接访问实例属性或调用实例方法，也无法直接访问类属性。它们独立于对象，只与类本身相关。
- 类方法：类方法可以访问和修改类属性，也可以调用其他类方法。但是类方法无法直接访问实例属性，因为它们没有与特定实例相关的上下文。

3. **使用场景：**

- 静态方法：静态方法适用于在类层级上执行某些操作，与类的实例无关，且不依赖实例属性或方法的情况下使用。
- 类方法：类方法适用于需要访问类属性或调用其他类方法的场景。它们通常用于实现与类本身相关的逻辑，例如创建工厂方法或进行类级别的操作。
# 💦 模块依赖
---
```python
# 导入模块
import pymongo

# 访问模块中的字段
print("version:", pymongo.version)
# 使用模块中的类
mongo_client = pymongo.MongoClient()
```

```python
# 导入类
from pymongo import MongoClient

# 直接使用导入的类
mongo_client = pymongo.MongoClient()
```