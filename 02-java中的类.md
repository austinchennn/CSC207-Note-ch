# 第2章：Java 中的类

## 2.1. 类

Java 中的类与 Python 中的类类似：它们由属性和方法组成，可以是私有的也可以是公共的。我们可以从其他类继承、重写方法以及定义构造函数。

虽然本节在较高层次上介绍了类，但代码文件 [Monster.java](code/Monster.java) 提供了一个带有内联说明的更大示例，可能会对你有所帮助。建议查看该文件以更好地理解我们使用的语法。

## 2.2. 类中的变量

与 Python 不同，Java 在使用变量之前必须先声明它们。我们可以为类声明两种变量：

1. **实例变量（Instance variables）**：类似于你在 Python 中熟悉的属性。类的每个实例都包含各自独立的实例变量副本。它们在用 `new` 关键字创建实例时诞生。
2. **类变量（Class variables）**：也称为**静态变量（static variables）**，一个类的所有实例共享*同一个*类变量实例。在某个实例中更新该变量，会反映到类的每一个实例上。这非常有用，例如当我们希望所有实例共同累积某个值时。

## 2.3. 可见性与访问修饰符

属性和方法可以是**public（公共）**或**private（私有）**的。在 Python 中，我们用变量名前面的单下划线（`_`）来表示属性是否私有。在 Java 中，我们使用 `public` 或 `private` 关键字来做出这种区分：私有变量*不能*从类的外部访问。我们还可以使用 `protected` 关键字，使属性或方法对整个包以及该类的任何子类都可访问，但对其他代码不可访问。如果不加任何访问修饰符，变量是"包保护的（package-protected）"，与 `protected` 相同，但不包括来自包外的子类访问。

关于 Java 访问修饰符的更多信息，可以参考：https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html

## 2.4. 构造函数

类似于 Python 中的 `__init__` 方法，Java 中有构造函数（constructor）。构造函数是没有返回类型（连 void 也没有）的方法，每次创建类的新实例时都会被调用。

在 Java 中，只要方法签名不同，我们可以定义任意多个构造函数：参数数量不同、参数类型不同，或两者兼而有之。

例如，以下是一个构造函数：

```java
public Something(String name, int size) {
    this.name = name;
    this.size = size;
}
```

在 Python 中，包括 `__init__` 在内的所有方法都将 `self` 作为第一个参数，`self` 是对调用该方法的对象的引用。在 Java 中，可以使用 `this` 关键字来代替。在实例方法或构造函数内部，`this` 是对当前正在执行的方法或构造函数所属对象的引用。注意，与 Python 不同，Java 中不需要将 `this` 作为参数写在方法签名中！

如果需要，还可以定义额外的构造函数。例如，如果我们想要一个只接受名称的构造函数：

```java
public Something(String name) {
    this.name = name;
    this.size = 1;
}
```

更好的方法是：重写上述构造函数，使其调用我们之前写的第一个构造函数：

```java
public Something(String name) {
    this(name, 1);
}
```

注意上面的语法，`this(name, 1)` 调用了之前定义的 `Something(String name, int size)` 构造函数。

### 2.4.1 对象创建顺序

在 Java 中使用 `new` 关键字创建新对象时，会按照特定顺序发生以下几个步骤：

1. 为对象分配内存。
2. 实例变量初始化为其默认值。
3. 直接初始化（如 `int x = 5;`）按照它们在类中出现的顺序执行。
4. 构造函数被调用，可能会进一步修改实例变量。
5. 如果该类继承自另一个类，超类的构造函数会*首先*被调用；我们将在下一章关于类之间关系的部分详细讨论这一点。

这个顺序确保在你的构造函数逻辑运行之前，所有继承的和已声明的字段都已被正确初始化。

> 这与 Python 类似，但区别在于 Python 中不会发生步骤 2 和步骤 3——等价于步骤 4 的 `__init__` 调用负责初始化所有实例属性。

## 2.5. 方法重载（Overloading）

当我们定义多个构造函数时，我们就是在**重载**构造函数。这同样适用于任何方法，不仅限于构造函数。在 Python 中，我们无法重载方法，但可以提供默认参数。例如：

```python
def my_method(self, something: int, something_else: int = 1) -> int:
    return something + something_else
```

调用 `my_method` 时，可以选择性地传入 `something_else` 的值，或者使用默认值 `1`。

在 Java 中，我们通过重载方法来实现类似的行为：

```java
public int my_method(int something, int something_else){
    return something + something_else;
}

public int my_method(int something){
    return my_method(something, 1);
}
```

注意，在上面的例子中，我们不必写 `this.my_method(something, 1)`，因为 Java 能够判断我们调用的是哪个方法。在这种情况下，`this` 是可选的。而之前在构造函数中写 `this.name = name` 时，`this` 是必需的，用于区分实例变量 `name` 和参数 `name`。

## 2.6. 方法重写（Overriding）

在 Python 中，我们可以在子类中重新定义父类的方法来覆盖它。Java 中做法类似，但还需要加上 `@Override` 注解。这让 Java（以及代码的读者）知道我们正在重写一个继承的方法。该注解还会强制检查我们使用了正确的方法签名——这样我们可以确定方法名没有拼写错误，参数类型也正确！

### 2.6.1. toString

我们经常想要重写的一个常见方法是 `toString` 方法。这是 Python 中 `__str__` 特殊方法的 Java 等价物：给出对象的字符串表示。

该方法不接受参数，返回一个 `String`。例如：

```java
@Override
public String toString(){
    return "My name is " + this.name;
}
```

### 2.6.2. equals

另一个从 `Object` 继承的常见方法是 `equals` 方法：这等价于 Python 的 `__eq__` 方法。注意，这个方法在 Java 中**不会**被隐式调用：`==` 检查的是身份相等（即两个对象的 ID 是否相同）。你需要显式调用 `equals` 方法（如 `object1.equals(object2)`）来检查相等性。

`equals` 的默认行为是检查身份相等（与 `==` 相同），但通常我们想要重写这个行为。

> 对于基本类型，由于只存储值，两个基本类型总是按值比较相等性。

类的设计者可以决定两个实例满足什么条件才被认为"相等"。这听起来实现起来很简单，但需要处理一些细节。任何实现都必须遵守以下属性：
1. **对称性（Symmetry）**：对于非 null 的引用 `a` 和 `b`，`a.equals(b)` 当且仅当 `b.equals(a)`
2. **自反性（Reflexivity）**：`a.equals(a)` 必须为 true
3. **传递性（Transitivity）**：如果 `a.equals(b)` 且 `b.equals(c)`，则 `a.equals(c)`

`equals` 方法通常如下所示：

```java
@Override
public boolean equals(Object obj) {
    if (this == obj){
        return true;
    }
    if (obj == null){
        return false;
    }
    if (this.getClass() != obj.getClass()){
        return false;
    }

    MyClass other = (MyClass) obj;  // 这里我们将 obj 的类型转换为我们的类
    // 然后我们要比较 this 的属性和 other 的属性
    // 例如：
    if (this.name != other.name){
        return false;
    } else if (this.something != other.something){
        return false;
    }
    return true;
}
```

可以在 `Monster.java` 中看到另一个例子。如果你将参数类型从 `Object` 改为 `Monster`，会发生什么？你应该会看到 IntelliJ 给出一些有用的提示，告诉你这会带来什么问题。

### 2.6.3. hashCode

每当我们重写 `equals` 方法时，通常也会想要重写另一个继承的方法——`hashCode`（哈希码）。对象的哈希码是一个整数值，满足以下属性：

> 如果两个对象相等（根据 `equals` 方法），它们具有相同的哈希码。

这不是充要条件：两个具有相同哈希码的对象*可以*不相等。

为什么对象要有哈希码，为什么它必须满足这个属性？一些重要的类（如 `HashMap`）使用对象的 `hashCode` 方法来决定在哪里存储它。这样在后续检索对象时，只需比较哈希码，避免了昂贵的 `equals` 方法调用。`HashMap` 类通过一种叫做"哈希表（hash table）"的数据结构实现这一切。哈希表具有出色的特性，在计算机科学中非常重要。你可以在 CSC263 等课程中深入学习它们。Python 中的字典就是你可能用过的依赖哈希映射来提供高效操作的一个例子。

如果我们没有在某个类中重写 `hashCode` 方法，那么默认的 `hashCode` 将是 `Object` 类中的那个。就我们而言，`Object` 生成的哈希码是随机的，我们不知道其值。

## 2.7. 类（静态）方法

就像我们有类（静态）变量一样，我们也可以有类方法。同样，使用 `static` 关键字来声明一个方法是类方法。由于类方法与类关联而非与实例关联，我们通过类名前缀来调用它。

例如，假设有以下类方法：

```java
public static int population(){
    return MyClass.count;
}
```

要调用它，我们写 `MyClass.population()`。如果有一个 MyClass 的实例 `m1`，也可以调用 `m1.population()`，但这看起来有些奇怪，因为 `population` 方法并不依赖于 `m1` 本身。

虽然实例方法可以引用类变量（或调用类方法），但反过来则不行。类方法**不能**直接访问实例变量或调用实例方法。

所以在我们的例子中，以下代码不会通过编译：

```java
public static int population(){
    return this.some_attribute;
}
```

在类方法中没有"this"！

类方法访问实例变量或调用实例方法的唯一方式是：通过传入某个类实例的引用。通过该引用，才能访问对象的实例变量和实例方法。

## 2.8 关键字 `final`

Java 中的 `final` 关键字用于限制修改，可以应用于变量、方法和类：

final 变量在初始化后不能被重新赋值：

```java
final int MAX_SIZE = 100;
```

例如，上面的 `MAX_SIZE` 不能被改变。这通常与 `static` 结合使用，用于定义常量。

注意，如果一个 `final` 变量引用的是一个对象，那么引用本身不能被改变，但它所指向的对象**仍然可以被修改**：

```java
final List<String> names = new ArrayList<>();
names.add("Alice"); // 允许——我们在修改对象
names = new ArrayList<>(); // 不允许——重新赋值是被禁止的
```

final 方法不能被子类重写，final 类不能被子类化。使用 `final` 有助于强制不可变性和设计约束，可能使你的代码更安全、更易于推理。
