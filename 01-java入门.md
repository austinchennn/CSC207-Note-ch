# 第1章：Java 入门

## 1.0 为什么要学 Java？

这是一门关于面向对象软件设计的课程，那为什么不继续使用你已经熟悉的 Python 呢？这是 CSC207 课程开始时的常见疑问！虽然我们可以继续使用 Python，但学习 Java 有以下几个好处：

- Java 仍然是工业界使用最广泛的编程语言之一，尤其在企业软件和大型系统中。
- 通过对比 Python 和 Java 的 OOP 实现，你能更深入地理解面向对象编程的基础。
- 在计算机科学的学习过程中，你会接触到许多编程语言。现在学习 Java 有助于培养快速上手新语言的能力。

在下一门课程 CSC209 中，你将接触更贴近硬件的 C 语言。Java 在抽象层次上介于 Python 和 C 之间——它仍然是高级语言，但引入了静态类型和手动编译等特性，为之后学习 C 语言打下基础。

在正式接触代码之前，我们先来了解一点 Java 的运行原理。

### 1.0.1 运行一个程序

先退一步思考："运行"一个程序究竟意味着什么？

当你用 Java 或 Python 等高级语言编写代码时，计算机无法直接理解。代码必须被翻译成最终在物理硬件上执行的机器码。

这种翻译主要有两种方式：

- **解释（Interpretation）**：程序由另一个叫做解释器的程序逐行读取并执行（如 Python）。
- **编译（Compilation）**：整个程序被编译器提前翻译成机器码，生成可执行文件（如 C）。

Java 采用了两者结合的混合方式。Java 编译器（`javac`）将源代码翻译成**字节码（bytecode）** 这一中间形式，再由 **Java 虚拟机（JVM）** 在运行时解释并优化这些字节码。

要运行一个 Java 程序，必须先**编译**再**执行**。

如果你有一个名为 `HelloWorld.java` 的简单程序，可以在终端运行 `javac HelloWorld.java` 进行编译，生成包含字节码的 `HelloWorld.class` 文件，然后通过 `java HelloWorld` 运行。

> 注意：如果 `javac` 不在你的系统 PATH 中，则需要一些配置才能直接运行此命令。

随着项目规模增大，你可能需要编译数百个源文件。直接运行 `javac` 会非常繁琐，因此 IntelliJ 等现代开发环境通过 Maven 或 Gradle 等**构建系统**自动化这一过程。这些工具负责管理编译、依赖和执行，让你专注于编写代码。不过，理解底层原理同样重要——尤其是如果你将来要学习 CSC209，那时你需要不依赖 IDE 来编译 C 程序。

### 1.0.2 计算机体系结构

正如我们刚刚了解的，Java 程序在运行前需要先编译。为了更好地理解 Java 的这种设计，我们来高层次地看一下计算机体系结构。

在早期课程（CSC108/148 和 CSC110/111）中，你专注于编写应用程序。但这些应用程序并不是独立运行的——它们依赖**操作系统（OS）**来管理资源并与硬件交互。操作系统充当程序与物理计算机之间的桥梁，负责内存管理、文件访问和进程调度等任务。

以下是体系结构的简化视图：

```
应用程序
---------------
操作系统
---------------
硬件
```

注意我们用**层次**来描述这种体系结构——贯穿整个课程，我们会深入探讨"层"这一基本概念。

像 C 这样的语言会被直接编译成在特定操作系统上运行的机器码，因此编译后的程序**不可移植**——必须为每个目标操作系统重新编译！

---

#### 1.0.2.1 虚拟机体系结构

**虚拟机（VM）**是一种模拟计算机的软件应用，它提供了一个一致的程序运行环境，与底层操作系统无关。

Java 这类语言通过虚拟机实现**可移植性**。为虚拟机编写的程序可以在_任何安装了相应虚拟机的系统_上运行。

我们可以在体系结构图中，在应用程序层和操作系统层之间加入虚拟机这一新层：

```
应用程序
---------------
虚拟机
---------------
操作系统
---------------
硬件
```

这种设计让开发者可以"一次编写，随处运行"——无需担心操作系统的差异。当然，虚拟机本身也是运行在操作系统上的应用程序，那些与操作系统相关的细节仍需处理，只不过现在全部由虚拟机负责。应用程序开发者可以专注于自己的应用，无需再操心这些底层细节。

---

#### 1.0.2.2 Java 体系结构

Java 的体系结构以**Java 虚拟机（JVM）**为核心，具备虚拟机的所有优点。Java 源文件被编译后，结果是存储在 `.class` 文件中的**字节码**，可由 JVM 执行。这意味着同一个 `.class` 文件可以在 Windows、macOS 或 Linux 上运行，只要安装了 JVM！

总结一下，我们的层次结构如下：

```
Java 应用程序
---------------
Java 虚拟机（JVM）
---------------
操作系统
---------------
硬件
```

通常我们只需关注编写 Java 源文件（`.java` 文件），但从全局视角理解源代码最终如何被执行是很有益的。上述内容也初步介绍了一些设计理念：用层次结构来组织系统，以及系统的不同部分承担各自独立的职责。

在其他 CS 课程中，你可以深入了解上述相关主题，例如 C 语言编程（CSC209）、硬件（CSC258）、操作系统（CSC369）、编译器（CSC488）和编程语言（CSC324）。

现在，让我们正式开始编写 Java 代码！

> 注意：在后续内容中，我们会展示相关的 Python 代码，并着重对比两种语言的异同，帮助你快速掌握 Java 语法。

## 1.1. 初识 Java

让我们从 Python 中最简单的一行代码开始：

```print(7 + 5)```

然后看看如何用 Java 实现。

### 1.1.1. 定义类

在 Java 中，所有代码都必须位于类中，且没有独立的函数，只有方法。因此，如果想计算并打印 `7 + 5`，我们需要先定义一个类，再在其中定义一个方法来放置代码。

下面是一个名为 `Hello` 的类的框架：

```java
class Hello {
    // 方法将放在这里。
}
```

注意花括号。Python 用缩进表示代码嵌套，而 Java 用花括号定义代码块，缩进不影响正确性。当然，缩进对程序员来说仍然很重要——它使代码更易读。

双斜杠表示该行的其余部分是注释。

### 1.1.2. 定义方法

我们需要将打印 `7 + 5` 的代码放在类的某个方法中，并希望能够运行该方法。因此，我们需要了解 Java 程序是如何运行的。

在 Python 中，我们可以在 shell 中运行单行代码，也可以运行整个模块。Python 模块就是单个文件中的代码，运行模块时代码从上到下依次执行。

Java 没有模块的概念，一切都围绕类来组织。在 Java 中执行程序时，实际上是执行一个类。一个类可能有多个方法，那么哪个方法会被执行呢？有一个特殊的方法叫做 `main`，任何类都可以定义它。如果运行某个类，`main` 方法就会被执行。

`main` 方法必须具有非常特定的签名才能被识别为这个特殊方法：

```java
class Hello { 
    public static void main(String[] args) {
        // 方法体将放在这里。
    }
}
```

这类似于 Python 的 `if __name__ == '__main__':` 块，但那是在*类之外*的。

关键字 `public` 决定了哪些代码、在哪里可以调用这个方法。Java 对于共享与隐藏类的数据成员和方法的哲学比 Python 更为谨慎，语言本身提供了强大的机制来精确控制类的访问范围。关于 `main` 方法签名其他部分的含义，你之后会学到。现在只需要知道：如果运行类 `Hello`，这个 `main` 方法就会被执行。

我们需要如此频繁地输入 `public static void main(String[] args)`，以至于 IntelliJ 为它提供了简写：`psvm`。

![IntelliJ 中的 psvm 补全](images/psvm.gif)

### 1.1.3. 打印内容

Python 用 `print` 函数打印内容，Java 则使用 `System.out.println` 方法：

```java
class Hello {
    public static void main(String[] args) {
      System.out.println(7 + 5);
    }
}
``` 

你可能会好奇这个方法为什么有这样的多部分名称。`System` 是一个类，`out` 是该类中定义的一个静态数据成员，它是另一个类的实例，该类有许多用于打印的方法，其中包括 `println`。我们读作"print line"（打印行），这个方法会在打印内容的末尾添加一个换行符。

**分号**是与 Python 的下一个不同之处。Python 语句在按下回车键时结束（除非添加反斜杠表示继续下一行）；Java 则用分号标记语句结束。

## 1.2. 变量与类型

### 1.2.1. 灵活的 Python vs. 严格的 Java

Python 在变量使用上非常灵活。看看这段 Python shell 的交互：

```python
>>> stuff = ['Jia', 'Musa', 'Vugar', 'Nicole']
>>> type(stuff)
<type 'list'>
>>> stuff = 14.6
>>> type(stuff)
<type 'float'>
>>> stuff = {12345: 'Jia', 55132: 'Vugar', 98765: 'Nicole'}
>>> type(stuff)
<type 'dict'>
```

你可能没有注意到我们做了多少"越界"的事：我们可以将任意类型的值赋给 Python 从未见过的变量 `stuff`，还可以随意改变它存储的值类型。这种自由很方便，但如果不仔细追踪 `stuff` 的类型，也很容易写出有 bug 的代码。这正是在 Python 中为函数定义类型契约如此重要的原因。

Java 则不同。与 Python 中可选的类型提示不同，Java 使用静态类型系统，类型声明是*必须的且由编译器强制执行的*。这种严格的类型检查确保我们遵守类型契约，有助于在编译时发现许多 bug，提高整体类型安全性。

### 1.2.2. 声明类型

在 Python 中，当我们使用 `type(stuff)` 时，得到的是 `stuff` 所指向对象的类型。变量 `stuff` 本身没有类型，它可以指向任意类型的对象。

在 Java 中，每个值都有类型，每个*变量*也有类型。我们必须在给变量赋值之前指定其类型，且类型永远不能改变。这叫做_声明_变量。举个例子：

```java
int i;
```

这里我们声明了一个名为 `i` 的 `int` 类型变量。内存中为该变量保留了空间，Java 记住了你承诺只给它赋 `int` 类型的值。

### 1.2.3. 声明与赋值

如果需要，我们可以在声明变量的同时立即赋值，甚至在同一行代码中：

```java
int i = 42;
```

在前面只写 `int i;` 的例子中，我们把给 `i` 赋值这件事推迟到了后面。在此期间，变量名已被 Java 识别，内存中已为其保留空间，并给了一个默认值。对于 `int`，默认值是 `0`；对于类类型，默认值是 `null`（相当于 Python 的 `None`）。

### 1.2.4. 追踪变量

Java 必须追踪与每个变量相关联的四件事：

1. 变量的名称，在声明变量时提供。
2. 变量的类型，也在声明变量时提供。
3. 用于存储变量值的内存空间。
4. 变量的值，可通过赋值语句来设定。

其中只有变量的值可以改变。

### 1.2.5. 错误

Java 会尽可能多地进行检查，以帮助我们避免 bug。以下是它能检测到的一些与变量和类型相关的错误：

#### 1.2.5.1. 未声明就使用

下面的代码使用了一个未声明的变量：

```java
public static void main(String[] args) {
    i = 42;
}
```

Java 会报错：`"i cannot be resolved to a variable."`（即变量 `i` 无法被识别）。这在 Python 中不会发生——Python 中给一个新名称赋值时，Python 会直接创建该变量。

不过别担心，如果你忘记声明变量，IntelliJ 会帮助你：

![IntelliJ 中的声明补全](images/declare.gif)

#### 1.2.5.2. 赋值类型错误

下面的代码给变量赋了错误类型的值：

```java
public static void main(String[] args) {
    int i = 19.6;
}
```

Java 报错：`"Type mismatch: cannot convert from double to int."`（类型不匹配：无法从 double 转换为 int。`double` 类型类似于 Python 的 `float`）。Java 发现了类型不匹配。这在 Python 中不会发生，因为 Python 中变量没有类型，只有对象才有。

注意，Java 尝试将 `19.6` 转换为变量 `i` 的类型，但做不到——这会导致精度损失。如果反过来，将 `int` 值赋给 `double` 变量，则可以转换，所以下面的代码完全没有问题：

```java
public static void main(String[] args) {
    double i = 19;
}
```

#### 1.2.5.3. 重复声明同名变量

下面的代码声明了一个名为 `i` 的变量，然后又声明了一次：

```java
public static void main(String[] args) {
    int i = 15;
    int i = 42;
}
```

Java 报错：`"Duplicate local variable i."`（重复的局部变量 i）。这在 Python 中不会发生——Python 中我们从不声明变量，只需使用它。第一次使用某个名称时，Python 创建该变量；下次使用同一名称时，Python 认为我们在引用同一个变量。

## 1.3. 引用类型与基本类型

### 1.3.1. 更多 Java 类型

目前我们见过一种变量类型：`int`。Java 还有很多其他类型，下面是一些简单的示例：

```java
// 我们已经见过 int 了。
int age = 21;
// Java 中的字符串必须用双引号，这与 Python 不同。
String name = "Jude";
// Java 中这个类型叫 boolean，而 Python 中叫 bool。
// Java 中的值是 true 和 false（首字母小写），而 Python 中是 True 和 False（首字母大写）。
boolean graduated = false;
// double 类型类似于 Python 的 float。
double gpa = 3.82;
```

还有其他整数类型和实数类型，允许我们节省内存（但降低精度）或提高精度（但消耗更多内存）。更多信息可以参考 [Java 官方教程](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)。

### 1.3.2. 引用类型 vs. 基本类型

在 Python 中，所有可以存储的值——哪怕是最简单的整数 `1`——都是对象。变量不会直接存储 `1`，而是存储一个指向包含值 `1` 的 `int` 对象的引用。你可能学过用唯一标识符（如 `id17`）来绘制内存模型图。在学习 Java 代码时，这些图依然非常有用。

然而，Java 不同。它有两种类型：_引用类型_和_基本类型_（primitive type）。理解二者的区别是写出正确 Java 代码的关键。

`String` 类型是**引用**类型——这和 Python 中的情况类似。Java 变量不能直接持有 `String` 值，只能持有一个指向 `String` 对象的引用。但 `int` 类型是**基本**类型，Java 变量可以直接在其内部持有 `int` 值。

方便的是，基本类型都以*小写字母*开头，引用类型以*大写字母*开头，因此很容易区分。

#### 1.3.2.1. 基本类型与引用类型在内存中的表示

假设我们运行以下代码：

```java
public class Simple {
    public static void main(String[] args) {
        int age = 21;
        String name = "Jude";
        System.out.println("Ciao!");
    }
}
```

Java 需要追踪许多内容才能运行代码：每个变量（名称、类型、内存位置和值）、所有已构造的对象，以及当前正在运行的方法。我们必须对内存中发生的事有清晰的认识，才能预测代码的行为。

让我们来看一下执行到 `println` 语句那一刻时计算机内存的状态：

![Java 内存模型图](images/1.3.png)

内存中有三个区域：

- **调用栈（Call Stack）**：用于追踪当前正在运行的方法。
- **对象空间（Object Space）**：用于存储对象。
- **静态空间（Static Space）**：用于存储类的静态成员（稍后详细介绍）。

调用栈中有类 `Simple` 的 `main` 方法的"栈帧"，该方法正在运行。栈帧中有我们的两个变量。`age` 直接在变量框内存储了值 `21`，因为 `int` 是基本类型。`name` 则没有直接存储 `"Jude"`，而是存储了一个指向包含值 `"Jude"` 的 `String` 对象的引用。由于我们没有编写 `String` 类的代码，不知道它用哪些数据成员来存储内容，所以直接在框内写 `"Jude"` 即可，这是完全合理的抽象。

基本类型与引用类型的区别在许多情况下都有影响，包括将值复制给另一个变量、向方法传递参数，以及比较两个变量是否相等等——稍后会详细介绍！

关于内存图还有一点值得注意：我们没有画箭头。程序员通常用箭头来表示一个东西引用另一个东西。当你对某种语言及其引用机制非常熟悉时，这样做很好。但在初学阶段，写下引用标识符（你可以自己编造 id 值）而不是画箭头，更容易做出正确的预测。

## 1.4. 字符串（String）

### 1.4.1. `String` 类

Java 有一个 `String` 类，表示字符序列。让我们创建一个新的字符串对象来表示文本 Hello：

```java
String s1 = new String("Hello");
```

注意，我们使用了**双引号**。这是 Java 中字符串字面量的必要格式。

Java 提供了创建字符串对象的快捷方式，不必显式使用 `new`：

```java
String s2 = "bye";
```

这种语法让你想起了 Python 中声明 `str` 的方式。不过，不要因为没有 `new` 就误以为 `String` 是基本类型——它是引用类型。

`String` 有很多可用的方法，更多信息可参见 Java API：https://docs.oracle.com/javase/8/docs/api/java/lang/String.html

### 1.4.1.1 字符串常量池（String Pool）

不过，使用 `new` 关键字创建 `String` 变量与不使用 `new` 之间存在区别。考虑以下 Python 代码：

```
>>> s1 = "Hello"
>>> s2 = "Hello"
>>> s1 == s2
True
```

最后的表达式结果为 `True`，因为两个字符串包含相同的字符序列。然而，Java 中有些不同。为了避免过多的内存消耗，Java 有一个特殊的"字符串常量池"来存储字符串字面量的值。例如，如果我们用 `String s1 = "Hello"`（不使用 `new`）创建一个 `String` 变量，Java 会自动将字符串 "Hello" 放入常量池。接下来如果用同样的方式创建 `String s2 = "Hello"`，为了节省内存，Java 会去常量池中查找这个值，发现 "Hello" 已经存在，于是让 `s2` 指向和 `s1` 相同的 "Hello" 对象。此时，`s1 == s2` 同样会是 `True`，和 Python 一样。

但如果我们这样写：

```java
String s1 = new String("Hello");
String s2 = new String("Hello");
System.out.println(s1 == s2);
```

结果将是 `false`！原因在于，我们创建了 `String` 类的两个独立_实例_。由于 `s1` 和 `s2` 指向不同的对象，结果是 `false`。总之，在比较 `String` 对象的值时，要注意这个细节，否则可能得到相反的结果。

上面描述的是字符串驻留（string interning）机制。关于字符串常量池和字符串驻留，可以参考以下链接：
1. https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#intern--
2. https://docs.oracle.com/javase/specs/jls/se8/html/jls-3.html#jls-3.10.5

### 1.4.2. 字符串是不可变的

与 Python 一样，Java 中的 `String` 对象也是不可变（immutable）的。这意味着我们永远无法修改一个已存在的 `String` 对象。我们*可以*对 `String` 对象进行操作，但这些操作不会修改原有的字符串，而是返回一个新的字符串。例如，参见 `concat` 方法的文档：https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#concat-java.lang.String-

### 1.4.3. 字符串操作与方法

`String` 对象可以拼接以产生新的 `String` 对象：

```java
// 我们不能修改 s1 或 s2，但可以用它们创建一个新的字符串。
String s3 = s1 + s2; 
```

`String` 类还有一组方法，包括：

```java 
// 索引
char c = s1.charAt(2);  // Python 中：s1[2]
// 切片
s1 = s1.substring(2, 4); // Python 中：s1[2:4]

// 去除空白
s1 = "   Here is my string  .   ";
s1 = s1.trim();  // Python 中：s1.strip()
```

还有许多有用的方法，包括 `length`、`startsWith` 和 `indexOf`。完整列表可查阅 Java 的 `String` 类在线文档：https://docs.oracle.com/javase/8/docs/api/java/lang/String.html

### 1.4.4. 可变字符串：StringBuilder

与 `String` 一样，`StringBuilder` 也表示字符序列；但 `StringBuilder` 对象是**可变（mutable）**的。Java 提供了许多用于修改 `StringBuilder` 的方法，示例如下：

```java
StringBuilder sb = new StringBuilder("ban");
// 我们不需要创建新对象来追加内容，可以直接修改 sb。
sb.append("phone");
// 其他一些方法也可以修改 StringBuilder：
sb.insert(3, "ana");
sb.setCharAt(3, 'o');
sb.reverse();
```

同样，可以参考 Java 的在线文档获取更多细节。

注意，我们没有写：

```java
StringBuilder sb = "ban";
```

这会产生错误：`incompatible types: String cannot be converted to StringBuilder`（不兼容的类型：String 无法转换为 StringBuilder）。

### 1.4.5. 单字符字符串：char

我们可以有一个只包含一个字符的 `String`：

```java
String s = "x";
```

还有一种基本类型可以持有单个字符，叫做 `char`。提供 `char` 类型的字面值时，必须使用单引号：

```java
char c = 'x';
```

`StringBuilder` 的 `setCharAt` 方法要求第二个参数是 `char` 类型，这就是我们在调用时使用单引号包裹 `'o'` 的原因：

```java
sb.setCharAt(3, 'o');
```

### 1.4.6. 修改字符串 vs. 创建新字符串

我们知道 `String` 是不可变的，而 `StringBuilder` 是可变的。如果我们每次需要修改字符串时都构造一个新的 `String`，也可以不用 `StringBuilder`。但构造新对象比修改已有对象要慢。例如：

```java
String a = "Joshua";
String b = "Giraffe";
a = a + b; // 构造一个新的 String——较慢。
```

```java
StringBuilder c = new StringBuilder("Baby");
StringBuilder d = new StringBuilder("Beluga");
c.append(d); // 直接修改——更快。
```

IntelliJ 在某些情况下甚至会提示你这一点并建议修改（更多循环语法稍后介绍）：

![IntelliJ 推荐使用 StringBuilder](images/stringbuilder.gif)

## 1.5. Java 中的类

虽然我们还没有开始定义自己的类（除了用于容纳 `main` 方法的简单类），但我们已经在使用 `String` 和 `StringBuilder` 等类了。下面我们来了解一些使用其他类的客户端代码所需的基本概念。下一章我们将深入学习如何定义自己的自定义类。

### 1.5.1. 抽象（Abstraction）

_抽象_是对复杂事物的简化视图。例如，在大一时你学过内存模型，这是程序运行时计算机内存工作方式的一种抽象。我们还可以进一步简化内存模型：一个箭头表示变量包含一个对象的内存地址（从而指向该对象），所以我们不需要写出那些内存地址——具体地址并不重要，有指针本身就足够了。这是抽象之上的抽象！

类和接口是另一种抽象。当我们编写操作数据的程序——税务信息、大学学生信息，或几乎任何东西——我们需要在程序中表示这些数据。例如，`Student` 类会包含姓名和学号，但通常不会包含体重和身高。这些细节与程序无关，因此不需要表示。`Student` 类就是对学生的一种抽象。

### 1.5.1. 实例化对象

在 Python 中，我们可以通过调用类的构造函数来创建类的实例（即创建对象）。例如，假设 Python 中有一个 `StringBuilder` 类：

```python
name = StringBuilder("Viriyakattiyaporn")
```

Java 中等价的代码需要使用关键字 `new`：

```java
StringBuilder name = new StringBuilder("Viriyakattiyaporn");
```

在括号中，我们为构造函数提供参数。一个类可以提供多个构造函数，编译器会根据参数的数量和类型来判断我们调用的是哪一个。

当 Java 执行表达式 `new StringBuilder("Viriyakattiyaporn")` 时，它会：

- 为新对象分配内存，
- 对参数进行求值，
- 调用合适的构造函数，
- 返回指向新构造对象的引用。

我们可以将引用赋给一个变量（如上所示），也可以直接使用它，例如：

```java
System.out.println(new StringBuilder("Professor").append(" Horton"));
System.out.println(new StringBuilder("Balakrishnan").indexOf("kris"));
```

### 1.5.2. API

现在我们有了一个指向某个类实例的引用，可以用它做什么？类的文档会告诉我们。如果是 `StringBuilder` 等内置 Java 类，应查阅 [Java 标准文档](https://docs.oracle.com/javase/8/docs/api/)，以获取可用方法和数据成员的完整信息。请现在去找到 [StringBuilder 的文档](https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html)。

文档大致如下：

![StringBuilder API 截图](images/1.5.png)

在该页面中，找到并查看接受 `String` 参数的构造函数、`append` 方法和 `indexOf` 方法的文档。

这份文档精确地规定了客户端代码如何与类交互（此处为 `StringBuilder` 类）。我们称之为_应用程序接口（API）_。它告诉我们可以调用哪些方法、必须传入哪些参数、以及会返回什么值。请务必收藏 Java API 并经常查阅。你会在网上找到各种 Java 资源，但 Java API 是最权威的参考。

注意，API 并不告诉我们类是如何提供这些服务的。当然，在一些例外情况下，描述实现是描述类运行时性能的最简单方式，但 API 文档通常聚焦于如何使用所提供的服务。

当我们只知道接口而不知道实现细节时，就可以以抽象的方式思考这个类，而不必深陷于实现细节中——在我们只是想_使用_这个类时，这些细节并不重要。

同时，类的实现者也可以自由地修改实现，而不会影响已有的客户端代码。只要 API 保持不变，一切都没问题。我们在定义辅助方法时也有同样的接口与实现分离，并获得同样的好处。

### 1.5.3. 调用方法

与 Python 一样，我们通过对类实例的引用来调用实例方法。例如：

```java
String band = "Arcade Fire";
// 通过实例调用实例方法。
int size = band.length();
```

你可以把它理解为"请求对象为你做某件事"。例如，当我们写 `band.length()` 时，就像在说"嘿，band，你是一个 `String`，告诉我你的长度！"

### 1.5.4. 类方法（静态方法）

有些方法不与类的单个实例关联，而是与整个类关联。我们称之为"类方法"（或"静态方法"，因为它们用 `static` 关键字定义）。通过类名来访问类方法。例如：

```java
// 通过类名调用类方法。
double x = Math.cos(48);
```

你可以把 `Math.cos(48)` 理解为"嘿，`Math` 类，告诉我 48 的余弦值"。

`String` 类有一些类方法，包括 `valueOf`，它接受一个 `int` 并返回对应的 `String`。`valueOf` 还有其他版本，可以将其他类型转换为 String——我们说 `valueOf` 被**重载（overloaded）**了，具有多种含义。以下是使用类方法 `valueOf` 的例子：

```java
int age = 12;
System.out.println("Age is " + String.valueOf(age));
```

"嘿，`String` 类，告诉我这个整数 age 对应的字符串值"是有意义的。但对某个具体的 `String` 对象 s 说"嘿，`s`，你是个字符串，告诉我这个整数 age 对应的字符串值"则显得有些奇怪。这正是类的设计者将 `valueOf` 设计为类方法而非实例方法的原因。

### 1.5.5. 访问数据成员

访问数据成员（也称为属性或实例变量）的方式与访问方法完全类似。

如果一个类有一个在类外可访问的实例变量或类变量（也称为 `static` 变量），可以分别通过实例变量或类名来引用。

例如，`Integer` 类有一个类变量 `BYTES`，表示存储一个 `int` 值所需的字节数。由于它是类变量，通过类名访问：

```java
System.out.println(Integer.BYTES); 
```

在类外可直接访问实例变量的情况比较罕见，因为实现细节通常是私有的。但如果有一个叫 `Sneetch` 的类，其中有一个公共实例变量 `motto`，我们可以这样写：

```java
Sneetch sam = new Sneetch();
System.out.println(sam.motto); 
```

还有，IntelliJ 的代码补全功能可以提示你哪些属性和方法是可用的：

![IntelliJ 代码补全示例](images/codecomplete.gif)

### 1.5.6. 回收不再使用的对象

构造对象时，内存会被分配；那么内存又是何时被释放的呢？在 C 语言等一些编程语言中，动态分配的内存只有在代码明确要求时才会被释放。然而，Java 有自动"垃圾回收"机制：它追踪所有对象，一旦确认程序中没有任何变量引用某个对象，就释放该内存，使其可供其他用途使用。

如果我们知道不再需要某个对象，可以将持有引用的变量设置为 `null`，从而显式地丢弃引用。这可以加快垃圾回收并提升性能，但也可能是不必要的，反而使代码变得冗余。对此感兴趣的人可以参考 [StackOverflow 上的讨论](https://stackoverflow.com/questions/449409/does-assigning-objects-to-null-in-java-impact-garbage-collection)。

## 1.6. 数组

数组是 Java 提供的最简单的集合类型，有点像 Python 的列表，但简单得多。具体来说：

- 数组有固定的长度，在构造数组时确定，之后永远无法改变。
- 所有元素必须是相同的类型，在声明引用数组的变量时就必须指定。实质上，变量的类型不只是"数组"，而是"int 数组"或"String 数组"之类的。

### 1.6.1. 声明数组

声明数组时，需要：
- 用方括号表示类型是数组，
- 在方括号前指定每个元素的类型。

按照惯例，两者之间不加空格。例如：

```java
int[] numbers;
```

这声明了一个 `int` 数组，读作"int 数组 numbers"。

数组是引用类型。这意味着声明 `numbers` 时，创建的是一个将引用某个数组对象的变量。我们并没有创建数组本身，只是创建了一个可以指向数组的变量。

### 1.6.2. 构造数组

要实际创建数组，我们需要用 `new` 关键字构造对象。这会调用合适的构造函数，根据给定的参数数量和类型来选择。对于数组，有一些特殊语法，看起来与普通方法调用略有不同——这种语法沿袭自 C 语言，使用**方括号**而非圆括号。唯一需要提供的参数是数组的**大小**。继续以 `int` 数组 `numbers` 为例：

```java
int[] numbers = new int[5];
```

现在对象空间中存在一个数组对象，该对象有 5 个位置，每个位置可以存放一个 `int`，默认值为 `0`，变量 `numbers` 引用整个对象。

在这个例子中，我们在编写代码时就确定了数组的大小。也可以在程序运行时通过读取输入或计算来决定大小，因为大小不是类型的一部分。例如，上面的 `numbers` 数组的类型只是 `int[]`，可以将任意长度的 int 数组赋给它。

#### 1.6.2.1. 赋值前的默认值

我们没有给这 5 个位置赋值，但它们会自动初始化为 `int` 的默认值 `0`。每种内置类型都有对应的默认值。

#### 1.6.2.2. 另一种构造数组的方式：使用初始化器

我们可以将数组对象的构造和初始化合并为一步：

```java
int[] numbers = {1, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024};
```

与所有赋值语句一样，右边的表达式先被求值。注意右边用花括号包裹值的语法。这会构造一个大小恰好合适的数组对象，然后按照对应的索引依次赋值。

### 1.6.3. 确定数组长度

可以通过访问数组的 `length` 属性来获取数组长度。下面用它来构造一个与 `numbers` 等长的新数组：

```java
int[] sibling = new int[numbers.length];
```

### 1.6.4. 访问数组元素

访问元素的语法与 Python 相同。例如，给索引 `1` 处的整数赋值：

```java
numbers[1] = 512;
```

由于 `int` 是基本类型，值 `512` 直接存储在数组中。

数组索引从零开始，与许多其他编程语言一样。因此上面的代码并没有把 `512` 放入第一个位置，而是放在了第二个位置。

Python 提供了一些花哨的索引方式，例如：

```python
houses = ["Hufflepuff", "Gryffindor", "Slytherin", "Ravenclaw"]
forHarry = houses[-3]   # 从末尾数第三个元素
enemies = houses[1:2]   # 通过"切片"复制部分元素
```

Java 数组**不支持**切片，也不允许负索引。如果尝试访问索引不在 0 到数组长度减一之间的元素，就会报错。例如：

```java
String[] houses = {"Hufflepuff", "Gryffindor", "Slytherin", "Ravenclaw"};
String forHarry = houses[-3];
```

会产生错误：`Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: -3`（数组越界异常）。

### 1.6.5. 为什么要有这么受限的类型？

我们看到数组比 Python 列表更受限，最大的限制是数组大小永远不能改变。

你可能会问，既然 Python 列表等更灵活的结构已经被发明出来，Java 为什么还要有数组？事实上，Java 也有更灵活的结构，包括 [ArrayList](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)，稍后会学到。但 Java 保留了数组，原因在于数组非常高效。支持任意增长和收缩的列表结构需要额外的空间和时间。因此 Java 给你选择：如果不需要这种灵活性，使用效率超高的数组；如果需要，则用 `ArrayList` 等灵活结构。

### 1.6.5.1. 通过继承在数组中混合类型

我们可以利用继承来绕过数组中所有元素必须相同类型的限制。每个 Java 类都是内置类 `Object` 的后代（类似于 Python 中所有类都继承自 `object`）。因此，我们只需声明数组存放 `Object` 类型的值，就可以放入任意类型的对象：

```java
Object[] miscellany = new Object[5];
// 可以将 String 对象放入数组。
miscellany[0] = new String("Songbird");

// 如果有一个叫 Monster 的类，也可以放入 Monster 对象！
miscellany[1] = new Monster("Fred");
// 虽然有特殊语法，但数组本质上也是 Object 的一种，
// 所以也可以放入数组。
miscellany[2] = new int[50];
```

这样我们就可以把各种类型的对象放入 `miscellany`。但当我们访问数组元素时，Java 只能从代码中知道它是一个 `Object`，这带来了一些后果：

```java
// 写这行时，Java 会检查类型是否匹配，一如既往。
// Java 知道 miscellany 中存放的是 Object，所以这个赋值没问题：
Object element = miscellany[0];
// 但这里，Java 不满足于仅知道我们在把一个 Object 引用放入 String 变量。
// 如果这个 Object 不是 String 怎么办？！所以这行会报错：
String s = miscellany[0];
```

当然，我们知道放在那个位置的是 `String`。我们可以通过**类型转换（casting）**来告诉 Java 这一点，即告诉 Java 将其视为 `String`，并隐含地承诺运行时它确实会是一个 `String`。对 `miscellany[0]` 进行强制类型转换的写法如下：

```java
// 在圆括号中写我们要转换的目标类型。
String s = (String) miscellany[0];
```

在运行时，如果我们转换的对象实际上不是字符串，就会产生运行时错误，例如：

```java
// 这次我们访问索引 1，里面是一个 Monster。Java 读代码时不报错，
// 但运行时发现要将 Monster 转换为 String，就会报错。
String s = (String) miscellany[1];
```

这会产生错误：`java.lang.ClassCastException: Monster cannot be cast to java.lang.String`（类型转换异常：Monster 无法转换为 String）。

### 1.6.6. 二维数组

我们也可以创建多维数组。例如，定义一个元素本身也是整数数组的数组：

```java
int[][] table;
// table 的类型是 int[][]（"int 数组的数组"）。
// 此时我们只有一个存放引用的变量，什么都没有。
table = new int[50][3];
// 现在我们有一个 50 元素的数组，每个元素引用一个 3 元素的 int 数组。
// 变量 `table` 引用整体。
// 需要区分两个维度的范围：
// 这样合法：
table[49][2] = 123;
// 这样不合法：
table[2][49] = 123;
```

#### 1.6.6.1. 不规则维度的数组！

注意，我们并没有真正创建一个矩形对象，而是创建了一个元素本身也是数组的数组。如果需要，可以分别创建这两层数组：

```java
int[][] table;
table = new int[50][];
// 现在我们有一个 50 元素的数组，每个元素可以引用一个 int 数组，但尚未引用任何东西。
for (int i = 0; i < 50; i++) {
    table[i] = new int[3];
}
// 现在我们和上面用 new int[50][3] 创建的结果一样了。
```

这种将两个维度解耦的方式让我们可以创建不规则形状的多维数组：

```java
int[][] irregular;
irregular = new int[3][];
irregular[0] = new int[6];
irregular[1] = new int[99];
irregular[2] = new int[10];
irregular[1][8] = 170;
```

## 1.7. 别名（Aliases）

### 1.7.1. 别名及其影响

我们已经知道，Java 有两种类型：基本类型（如 `int`）直接在变量内存储值，引用类型（如 `String`）在变量内存储一个指向对象的引用，需要追踪引用才能访问对象中存储的值和方法。

为了理解代码的行为，我们必须时刻清楚变量是基本类型还是引用类型。这会从根本上影响代码的执行结果。

### 1.7.2. 引用可以创建别名

只要创建了一个对象，就有可能让两个不同的变量引用同一个对象。例如：

```java
String name = new String("Justin Trudeau");
String primeMinister = name;
```

假设这两行出现在 `Demo` 类的 `main` 方法中。执行后，内存状态如下：

![Java 中别名的内存模型示例](images/1.7-1.png)

我们有两个变量引用同一个 `String` 对象。当两个变量引用同一个对象时，我们称它们是**别名（aliases）**。

对比一下字典里"alias"的定义！别名用于一个人以不同的名字为人所知，例如"Eric Blair，别名 George Orwell"。两个名字指的是同一件事，在这里是同一个人。

在 Python 中，我们同样可以以这种方式创建别名：

```python
name = "Justin Trudeau"
primeMinister = name
```

### 1.7.3. 基本类型无法创建别名

回到 Java，如果用 `int` 代替 `String` 写类似的代码，_不会_创建别名：

```java
int number = 42;
int answer = number;
```

![Java 中无别名的内存模型示例](images/1.7-2.png)

没有对象被创建，更重要的是没有引用被创建。变量 `number` 和 `answer` 各自持有自己的 42。

注意，Python 中无法产生相同的情况，因为它没有基本类型。

### 1.7.4. 别名的副作用

与 Python 一样，Java 中的别名也会产生副作用。如果有两个引用指向同一个对象，必须清楚这一点，否则代码会做出出乎意料的事。

假设有一个叫 `Monster` 的类，它有一个 `grow` 方法（使怪兽变大）和一个 `size` 方法（获取怪兽的大小）：

```java
// 创建一个大小为 12 的 Monster。
Monster one = new Monster(12);
Monster two = one;
// 增大 Monster two 的大小。
two.grow();
// Monster two 的大小现在是 24。Monster one 有多大？
int n = one.size();
```

根据你对内存中情况的理解，你可能会回答 `12` 或 `24`。调用 `grow` 之前内存的实际状态如下：

![Monster 别名示例（第1部分）](images/1.7-3.png)

调用后的状态：

![Monster 别名示例（第2部分）](images/1.7-4.png)

由于 `one` 和 `two` 是别名，改变 `two` 也会影响 `one`。更准确地说，改变 `two` 所引用的对象，就是改变 `one` 所引用的对象——因为它们是同一个对象！

### 1.7.4.1. 但前提是对象是可变的！

假设我们对不可变对象创建了别名，如前面的例子：

```java
String name = new String("Justin Trudeau");
String primeMinister = name;
```

由于 `String` 对象不可更改，就不可能产生副作用。我们能做的最多是创建一个新的 `String` 对象：

```java
String name = new String("Justin Trudeau");
String primeMinister = name;
primeMinister = primeMinister.replace('u', 'U');
System.out.println(name);
System.out.println(primeMinister);
```

这段代码的输出是：

```
Justin Trudeau
JUstin TrUdeaU
```

我们并没有通过修改 `primeMinister` 来改变 `name`。事实上，我们并没有改变 `primeMinister` 所引用的 `String`——我们创建了一个**新的** `String`。

### 1.7.5. 通过复制避免副作用

我们知道，当两个变量是同一个可变对象的别名时，会产生副作用。为了避免副作用，可以复制对象而非创建别名：

```java
String[] words = {"hello", "bonjour", "adieu", "tschuss", "ciao"};
// 这看起来像复制，但实际上不是，它只是一个别名。
String[] copy1 = words;
copy1[1] = "xox";
String[] copy2 = new String[words.length];
for(int i = 0; i < words.length; i++) {
    copy2[i] = words[i];
}
copy2[3] = "yoy";
```

如果打印 `words`、`copy1` 和 `copy2`，结果是什么？有了对内存中情况的清晰认识，这个问题就容易回答了：

![副本的内存模型图](images/1.7-5.png)

注意，尽管名字叫 `copy1`，它实际上并不引用数组的副本，只是对原有数组对象的第二个引用。对 `words` 引用的数组的修改会体现在 `copy1` 中（反之亦然），因为它们是*同一个数组*。但 `copy2` 引用了一个独立的数组对象，其中每个元素都是第一个数组对应元素的复制品。修改 `copy2` 不会影响原始数组 `words`。

### 1.7.6. 浅复制 vs. 深复制

上面我们做了所谓的*浅复制（shallow copy）*。我们把 `words[0]` 中存储的引用复制到 `copy2[0]`，把 `words[1]` 中存储的引用复制到 `copy2[1]`，依此类推。但我们并没有复制 `words[0]`、`words[1]` 等所引用的对象。结果是，尽管 `words` 和 `copy2` 是两个独立的数组，但它们各自指向同样的五个 `String` 对象。

换句话说，别名存在于更*深*的层次上。例如，`words[4]` 和 `copy2[4]` 都引用同一个 id 为 `id14` 的 `String`。

由于字符串对象不可变，这种更深层次的别名不会造成同样的问题。但如果复制的是一个由可变对象（如数组或 `Monster` 对象）组成的数组，就可能出现这些问题：

```java
int[][] table = {
    {11, 22},
    {5, 10}
};
int[][] copy = new int[2][2];
for (int i = 0; i < table.length; i++) {
    copy[i] = table[i];
}
int[] row = {0, 0};
copy[0] = row;
// 修改 copy[0] 没有影响 table，一切正常。
// 那这样做呢？
copy[1][1] = -99999;
```

要预测结果，同样需要对内存中的情况有清晰的认识：

![浅复制内存模型图](images/1.7-6.png)

所以，修改 `copy[1][1]` 确实也改变了 `table`：它的 `[1][1]` 处的值变成了 `-99999`。为了避免这种情况，我们必须在*每一层*都进行复制，这被称为**深复制（deep copy）**。

### 1.7.7. 其他类型的副作用

向方法传递参数时，也可能产生这类副作用，有时是我们希望的，有时则不是。

回想一下 Python 中传入列表作为参数时的情况：对传入的列表进行修改，原列表也会随之改变！Java 中的概念完全相同。

## 1.8. 控制结构

Python 使用缩进来表示嵌套代码块。以下面的代码为例：

```python
class_size = 124
sections = 1;
if class_size > 100:
    sections = 2
    class_size = class_size / 2;
print('Sections: {} and class size: {}'.format(sections, class_size))
```

我们可以清楚地看到哪些是 if 语句的一部分（缩进的行），哪些不是（初始变量赋值和 if 语句后的 `print` 语句）。

在 Java 中，空白和缩进不影响程序行为（但对可读性非常重要！）。Java 使用花括号 `{}` 来定义代码块的结构。

### 1.8.1. if 语句

Java 的 `if` 语句与 Python 非常相似，但有一些额外的语法。最简单的 `if` 语句只有一个条件和对应的主体：

```java
int classSize = 124;
int sections = 1;
if (classSize > 100) {
    sections = 2;
    classSize = classSize / 2;
}
```

Java 中条件周围的圆括号是**必须的**！主体本身用花括号括起来。不过，如果 if 语句的主体只有一行，花括号是可选的：

```java
if (classSize > 500)
    System.out.println("Wow, that's big!");
```

但这样做有风险！如果添加更多代码，我们可能会因为缩进而误以为它在 if 块内：

```java
if (classSize > 500)
    System.out.println("Wow, that's big!");
    sections = 3;               // 这些行在 if 块之外！
    classSize = classSize / 3;  // 它们会一直执行！
```

但对 Java 来说，这些额外的行**在 if 块之外**，无论条件是否成立都会执行。因此，即使目前不需要，也应该始终在代码块上使用花括号。

与 Python 类似，if 语句可以有一系列额外条件，最后可以跟一个 else。含义与 Python 相同，但注意 Java 中用 `else if` 而不是 `elif`：

```java
int grade = 86;
char letterGrade;
if (grade > 80) {
    letterGrade = 'A';
} else if (grade > 70) {
    letterGrade = 'B';
} else if (grade > 60) {
    letterGrade = 'C';
} else if (grade > 50) {
    letterGrade = 'D';
} else {
    letterGrade = 'F';
}
```

当然，if 语句也可以嵌套：

```java
boolean precipitation = true;
boolean freezing = false;
if (precipitation) {
    if (freezing) {
        System.out.println("穿靴子！");
    } else {
        System.out.println("带上雨伞！");
    }
}
```

记住，是**花括号**而非缩进决定了 else 与内层（而非外层）if 条件的关联。

> **重要**：与 Python 不同，Java 没有 `and`、`or` 和 `not` 运算符。Java 中等价的运算符分别是 `&&`、`||` 和 `!`。

### 1.8.2. for 循环

基本 for 循环的语法来自 C 语言。C 语言现在已经相当古老了，这种语法感觉很"手动"。

基本 for 循环的通用结构：

```
for (初始化; 终止条件; 递增) {
    循环体
}
```

for 循环的头部由三部分组成：
- *初始化*：在任何迭代开始之前执行一次，通常将计数器设为 0，但可以是任何语句。
- *终止条件*：布尔条件，为 true 时执行循环体和递增操作。
- *递增*：通常是递增某个变量，但也可以是任何语句。

下面是一个简单的例子，求前 `n` 个数的和：

```java
int n = 15;
int sum = 0;
for (int i = 1; i <= n; i++) {
    sum += i;
}
System.out.println("前 " + n + " 个数的和是 " + sum);
```

三部分分别是：
- `int i = 1`：*初始化*，声明并初始化变量 `i` 为 1。
- `i <= n`：*终止条件*，只要 `i <= n` 为 true 就循环，否则终止。
- `i++`：*递增*，等价于 `i += 1`：每次迭代将 `i` 增加 1。

我们从 `1` 数到 `n`（含）（因为终止条件是 `i <= n`）。注意，初始化中包含了变量声明，这限制了 `i` 的作用域（代码中可以引用它的范围）仅在循环内部。循环结束后，`i` 从栈帧中消失，保持命名空间整洁。

下面是一个使用数组的例子：

```java
int[] powers = {1, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024};
for (int i = 0; i < powers.length; i++) {
    System.out.println(powers[i]);
}
```

注意，我们从 `0` 数到 `powers.length - 1`，这正好是数组所有合法的索引范围！

#### 1.8.2.1. 增强型 for 循环

Java 还提供了一种增强型 for 循环，更接近于 Python 中的习惯。它可以用于数组和"集合"（稍后会学到）。

下面是一个使用数组的例子：

```java
for (int p : powers) {
    System.out.println(p);
}
```

这类似于 Python 中的：

```python
for p in powers:
    print(p)
```

这种循环比使用普通 for 循环简单得多，因此不容易出错。应尽可能使用增强型 for 循环。

#### 1.8.2.2. 题外话：简写操作符（++, --, +=, -=）

你可能记得 Python 中的快捷方式，如 `+=` 和 `-=`：

```python
x += n    # 等价于：x = x + n
y -= n    # 等价于：y = y - n
```

Java 也有这些，还额外提供了两种：

```java
i++;      // 等价于：i = i + 1
i--;      // 等价于：i = i - 1
```

这些很方便，`i++` 经常出现在 for 循环的递增部分。

但这些简写还有其他用法，有时难以阅读，容易引发 bug。例如，Java 同时支持 `++i` 和 `i++`，二者都会将 `i` 增加 `1`，但在表达式中使用时行为不同：

- `++i` 是*前置递增*：值先增加，再使用。
- `i++` 是*后置递增*：值先使用，再增加。

在更复杂的表达式中，这种区别很重要，如果不小心可能导致微妙的 bug。

### 1.8.3. while 循环

Python 中 while 循环的语法是：

```python
while condition:
    ...
```

Java 中 while 循环的形式类似：

```java
while (condition) {
    ...
}
```

一个更完整的例子：

```java
int number = 37;
int divisor = 7;
while (number > divisor) {
    number = number - divisor;
}
// 到这里我们知道 (n > divisor) 为 false。
System.out.println("余数：" + number);
```

与 if 语句一样的结构规则：
- 条件必须在圆括号内。
- 如果 while 循环体超过一行，需要花括号；即使只有一行，也应该加花括号。

while 循环的含义与 Python 完全相同：每次到达循环顶部时对条件求值，为 true 则执行循环体并返回顶部，循环终止时条件必为 false。

#### 1.8.3.1. do-while 循环

do-while 循环是 Java 中 while 循环的另一种形式。

do-while 循环在循环运行完*之后*才检查条件，确保循环至少执行一次。语法如下：

```java
do {
    // 循环体
} while (condition); // 注意块结尾的分号。
```

while 循环还有另一种涉及分号的用法，与递增/递减运算符有关。记住，`i++` 是在检查条件后递增，而 `++i` 是在检查条件前递增：

```java
int i = 0;

while (i++ < 10);
```

这种 while 循环形式可以用来递增/递减数组索引，直到满足条件：

```java
int [] A = new int [] {2, 4, 6, 8, 10, 12};

int i = 0; 

while (A[i++] < 7); 
```

这种写法看起来可能不熟悉或令人困惑，不建议使用。这里仅供参考。

## 1.9. 参数

让我们快速回顾一些 Java 和 Python 共有的参数概念和术语。

下面是一个简单的例子：

```java
public static void messAbout(int n, String s) {
    // 方法内容省略。
}

public static void main (String[] args) {
    int count = 13;
    String word = new String("nonsense");
    messAbout(count, word);
}
```

在方法声明中，括号里定义的每个变量都叫做**参数（parameter）**。这里，`n` 和 `s` 是方法 `messAbout` 的参数。调用方法时，括号里的每个变量叫做**实参（argument）**。在这次调用 `messAbout` 时，实参是 `count` 和 `word`。

调用方法时，发生以下步骤：

1. 一个新的栈帧被压入调用栈。
2. 每个参数在栈帧中被定义。
3. 每个实参中包含的值被赋给对应的参数。

然后执行方法体。当方法返回时（无论是执行到 `return` 语句还是到达方法末尾），该方法调用的栈帧从调用栈中弹出，其中定义的所有变量——包括参数和局部变量——都消失了。

上面第 3 步的措辞暗示，所谓的"参数传递"本质上就是赋值。在上面的例子中，相当于执行了：

```java
n = count;
s = word;
```

如果方法的实参是一个变量，赋给方法参数的就是该变量中包含的值。让我们在不同场景下看看这有什么影响。

### 1.9.1. 传递基本类型

以下代码输出什么？

```java
static void increase(int i) {
    i = i + 1000;
}

public static void main(String[] args) {
    int cost = 14;
    increase(cost);
    System.out.println(cost);
 }
```

答案是 `14`。通过观察内存中发生的事，这个行为变得清晰。下面的图展示了方法将新值赋给 `i` 之前的程序状态：

![参数的内存模型图](images/Java-1-13-1.png)

调用栈中有两个变量，每个都存储值 `14`。变量 `cost` 和参数 `i` 各自都包含 `14`，修改其中一个显然不会影响另一个。

等价的 Python 程序会产生相同的输出：

```python
def increase(i: int) -> None:
    """将 i 增加 1000。"""
    i = i + 1000

if __name__ == '__main__':
    cost = 14
    increase(cost)
    print(cost)
```

但原因不同。你能画出这段代码的内存模型图，并解释为什么它同样无法增加 `cost` 的值吗？图会和 Java 版本*不一样*。

如何通过方法来改变 `cost` 的值？在两种编程语言中，方法相同：返回修改后的值，由调用代码将其赋给希望修改的变量。

Java 版本：

```java
static int increased(int i) {
    return i + 10;
}
public static void main(String[] args) {
    int cost = 9;
    cost = increased(cost);
    System.out.println(cost);
}
```

注意方法名改为 `increased`。现在方法有返回值，用名词而非动词更自然。`cost = increased(cost)` 读起来也很顺畅。

### 1.9.1. 传递引用会创建别名

我们已经知道，如果实参是一个变量，赋给方法参数的就是变量框内的值。如果变量是引用类型，框内的内容是一个引用，那么实参和参数就成了别名。接下来会发生什么取决于对象是否可变。

#### 1.9.1.1. 传递可变对象的引用

如果我们传递一个*可变*对象的引用，就有产生副作用的可能。这与前面学习别名时看到的副作用本质上是一样的。

副作用不一定是坏事：方法通常就是为了产生副作用而设计的。关键是你必须清楚自己处理的是什么（基本类型、不可变对象还是可变对象），才能确保代码按预期工作。

下面是一个传递可变对象引用的例子：

```java
static void increase(StringBuilder sb) {
    sb.append(sb);
    sb.append("!");
}
public static void main(String[] args) {
    StringBuilder word = new StringBuilder("rah");
    increase(word);
    System.out.println(word);
}
```

方法 `increase` 即将返回前的内存状态：

![可变参数别名内存模型](images/Java-1-13-2.png)

调用栈中有两个变量，都引用同一个 `StringBuilder` 对象。

一些值得注意的点：

- 调用方法 `increase` 时，一个新的栈帧被压入调用栈。
- 栈帧左上角写方法名，右上角写方法所属的类（这里是 `Parameter` 类）。
- 参数 `sb` 存在于栈帧中，调用方法时创建，方法返回时随栈帧一起弹出。
- 将实参 `word` 传给参数 `sb` 时，相当于把 `word` 框内的值（即 `id29`）赋给 `sb`，创建了别名。
- `id29` 是一个指向可变 `StringBuilder` 对象的引用。通过 `sb` 访问并修改该对象时，`word` 所引用的对象也随之改变——因为它们是同一个对象！

#### 1.9.1.2. 传递不可变对象的引用

如果传递的是不可变对象的引用，无论对参数做什么，都不会影响方法外部。这也不一定是坏事，取决于你的需求。

下面是一个例子：

```java
static void increase(String s) {
    // 我们无法调用方法追加到 s，因为 String 没有这样的方法。但可以用拼接运算符。
    s = s + s + "!";
}
 

public static void main(String[] args) {
    String sound = "moo";
    increase(sound);
    System.out.println(sound);
```

这段代码只打印普通的 `moo`。原因是，虽然和之前一样创建了别名，但我们无法（也无需）修改 `sound` 和 `s` 所引用的对象——我们创建了一个新对象。方法返回前内存的状态如下：

![不可变参数别名内存模型](images/Java-1-13-3.png)

如果想改变实参，解决方案与传递基本类型时相同：返回新值，由调用代码将其赋给实参：

```java
static String increased(String s) {
    return s + s + "!";    
}
 
public static void main(String[] args) {
    sound = "oink";
    sound = increased(sound);
    System.out.println(sound);
}
```

这段代码打印出 `oinkoink!`

#### 1.9.1.3. 混合可变与不可变对象的复合对象

当我们拥有包含其他对象的对象时，情况会变得更加复杂。关键在于：清楚地知道你传递的是基本类型还是引用类型，以及每一层结构上的对象是否可变。内存模型图提供了一种简洁的视觉方式来表示这些情况。
