# 第5章：Java 的陷阱与细节

本章将重点介绍一些在初学 Java 时容易被忽视的方面。

## 5.1. 变量遮蔽（Shadowing）

变量遮蔽发生在两个不同作用域使用了同一个变量名时。在 Python 中，一个例子是：

```python
def f() -> None:
    x = 10

    def g() -> None:
        x = 20
        print(f"g() 的 x = {x}")
    
    print(f"f() 的 x = {x}")
```

运行 `f()` 会打印以下内容：

```
g() 的 x = 20
f() 的 x = 10
```

每个函数在调用栈上都有自己的栈帧，`x` 变量就存储在其中。`f()` 的栈帧包含值为 `10` 的 `x` 以及函数 `g()` 的定义；`g()` 的栈帧则只包含值为 `20` 的 `x`——`g()` 执行完毕后，该栈帧消失，`f()` 的栈帧保持不变。你可以在 PyCharm 中打开代码并使用调试器逐步执行来亲眼看看。

Java 中有类似的概念。例如，考虑以下代码：

```java
public class ShadowExample {
    private int shadowedVariable = 10;

    public void shadowingMethod(){
        int shadowedVariable = 20;
        System.out.println(shadowedVariable);
        System.out.println(this.shadowedVariable);
    }
}
```

调用 `ShadowExample.shadowingMethod()` 时，会打印：

```
20
10
```

由于两个变量同名，我们必须用 `this` 来引用实例变量，而直接写 `shadowedVariable` 则引用局部变量。这与我们之前在构造函数中看到的情况类似：当参数名与实例变量同名时，需要使用 `this.name = name` 来区分。

## 5.2. 数组复制

在 Python 中，我们可以通过切片来复制列表。例如：

```python
lst = [1, 2, 3]
lst_copy = lst[:]
lst_alias = lst
```

`lst` 和 `lst_copy` 包含相同的元素，但有不同的内存地址：修改一个不会修改另一个。而 `lst_alias` 是 `lst` 的别名：修改其中一个就会修改另一个。

同样的概念适用于 Java，只不过数组有一个 `clone` 方法。注意我们切换到了 Java 的命名规范：

```java
int[] lst = {1, 2, 3};
int[] lstCopy = lst.clone();
int[] lstAlias = lst;
```

`lst` 与 `lstCopy`、`lst` 与 `lstAlias` 之间的关系，与 Python 示例中完全相同。

此外，Python 中嵌套列表的行为与 Java 中嵌套数组的行为相同。在 Python 中，如果有：

```python
nested_lst = [[1, 2], [3, 4]]
nested_lst_copy = nested_lst[:]
nested_lst_copy[1] = [5, 6]
nested_lst_copy[0][0] = 7
```

那么内层的嵌套列表会是别名，但外层列表不会。在这个例子中，每个列表的内容如下：

```python
>>> nested_lst
[[7, 2], [3, 4]]
>>> nested_lst_copy
[[7, 2], [5, 6]]
```

要做一个没有任何别名的深层复制，需要对每一个内层列表也进行复制。

Java 的行为完全相同：使用 `clone()` 会创建最外层数组的副本，但不会复制内层数组。要做深层复制，需要对所有内层数组也调用 `clone()`。

## 5.3. 自动装箱（Autoboxing）

在 Java 中，我们必须定义类型并遵守类型声明，否则代码无法编译。然而，**自动装箱（autoboxing）**是 Java 编译器在基本类型与其对应的对象包装类之间自动进行的转换，反之亦然（例如 `int` 和 `Integer`）。从包装类转换为基本类型称为**拆箱（unboxing）**。

例如，我们可以这样写：

```java
int x = 4;
Integer y = new Integer(x); // 等价于 Integer y = x
int z = y;
```

在第二行赋值中，我们可以直接写 `Integer y = x;`，值 `4` 会被自动装箱！在第三行，`y` 被拆箱，`z` 得到值 `4`。借助 Java 的自动装箱特性，我们可以直接使用基本类型，让 Java 在需要时自动进行装箱和拆箱，从而简化代码编写。

### 5.3.1. 值比较

考虑比较 `int` 值和 `Integer` 对象。如果**某个操作数是基本类型**，Java 会**拆箱** `Integer` 并比较值：

```java
Integer a = 100;
int b = 100;

System.out.println(a == b); // true —— 比较值；a 被拆箱
```

这是合理的，因为如果 `b` 被自动装箱的话，接下来会出现一个意外——我们马上就会看到！

### 5.3.2. 引用比较

如果**两个操作数都是 `Integer` 对象**，`==` 比较的是**引用**而非值：

```java
Integer x = new Integer(100);
Integer y = new Integer(100);

System.out.println(x == y); // false —— 不同的对象
System.out.println(x.equals(y)); // true —— 相同的值
```

这可能是 bug 的来源，因此正如我们在字符串和一般对象中看到的，比较对象时应始终使用 `equals`。好在 IDE 会给出警告，因为这是非常常见的 bug 来源。

### 5.3.3. Integer 缓存

与我们之前看到的字符串驻留类似，Java 会缓存 **-128 到 127** 范围内的 `Integer` 值以提高效率：

```java
Integer a = 100;
Integer b = 100;
System.out.println(a == b); // true —— 相同的缓存对象

Integer c = 200;
Integer d = 200;
System.out.println(c == d); // false —— 未缓存
```

### 5.3.4. Null 安全

对 `null` 的 `Integer` 进行拆箱会在运行时抛出 `NullPointerException`：

```java
Integer a = null;
int b = 100;

System.out.println(a == b); // 抛出 NullPointerException
```

为了比较 `a` 和 `b`，如上所述，`a` 会被拆箱。对 `Integer` 进行拆箱相当于在编译时将 `a` 替换为 `a.intValue()`。

> 如果你运行这个例子，错误信息会揭示具体错误：
> `java.lang.NullPointerException: Cannot invoke "java.lang.Integer.intValue()"
> because "a" is null`（无法调用 intValue()，因为 a 为 null）

关于自动装箱以及每种基本类型对应的包装类的更多详情，请参阅[官方 Java 教程](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html)！

你可能会想，既然使用包装类不小心会引发 bug，它们存在的意义是什么？下一章关于 Java 泛型的内容将会解答这个问题。
