# 第4章：Java 中的图形用户界面

现在我们已经熟悉了 Java 编程的基础，接下来迈向制作可视化 Java 应用程序的下一步：创建基本的用户界面。

这是对 Java Swing 的简要介绍，Swing 是 Java 中的图形用户界面（GUI）库之一。

许多示例参考了官方 Java 教程：https://docs.oracle.com/javase/tutorial/uiswing/

各种可视化类定义在 `javax.swing` 包中。该包中的大多数类名以 `J` 开头：例如，可点击的按钮定义在类 `javax.swing.JButton` 中。

所有可视化对象都叫做**组件（component）**。例如，`JPanel` 是一个可以包含其他组件的容器，其中可以包含 `JButton`，甚至其他 `JPanel`。

让我们来探索基础知识！

> 创建 GUI 时，你所编写的大部分代码都是_客户端代码_——你通过创建 Java Swing 类的实例并调用其方法来构建 GUI。要高效地做到这一点，需要养成阅读文档的习惯。学会自如地探索文档、学习新工具，是你整个软件开发生涯中都会用到的核心技能。

## 4.1 在 Java 中创建并显示窗口

<img src="images/MainFrameExample.png" align="right" width="300px" style="margin:16px;"/>

以下代码：

* 创建一个窗口（由类 `javax.swing.JFrame` 定义）
* 设置窗口的最小尺寸
* 设置点击关闭按钮时退出程序
* 根据所做的布局选择对内容（目前为空）进行打包
* 通过将可见性设为 `true` 来显示窗口

```java
JFrame frame = new JFrame("Intro JFrame Example");
frame.setMinimumSize(new java.awt.Dimension(300, 200));
frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
frame.pack();
frame.setVisible(true);
```

这里有一点涉及安全和线程的"魔法"，我们暂时还没有足够的工具来解释，但需要告诉 Java 按照下面的方式来创建和显示窗口：

```java
SwingUtilities.invokeLater(() -> {
    // 你想要执行的任何代码
});
```

请阅读 [MainFrame.java](code/gui/MainFrame.java) 中的完整示例。

接下来，我们将探索一些可以添加到 `JFrame` 中用于显示和收集信息的组件。

---

## 4.2 Java Swing 可视化组件

<img src="images/ButtonClickExample.png" align="right" width="300px" style="margin:16px;"/>

右边的窗口包含以下组件：

* `First Name:` 和 `Last Name:` 由 `JLabel` 类定义（使用时需要导入 `javax.swing.JLabel`）
* 文本输入框由 `JTextField` 类定义
* `Submit` 和 `Cancel` 由 `JButton` 类定义

有一种叫做 `JPanel` 的容器，可以水平（默认）或垂直排列，并包含其他组件，用来组织用户界面。

* `First Name:` 和它的 `JTextField` 在一个 `JPanel` 中
* `Last Name:` 和它的 `JTextField` 在一个 `JPanel` 中
* 两个按钮在一个 `JPanel` 中
* 这 3 个 `JPanel` 一起放在另一个 `JPanel` 中，我们称之为主面板

整个窗口由 `JFrame` 类定义。外层的 `JPanel` 放在 `JFrame` 的_内容面板（content pane）_中。

下面我们创建 First Name 的 `JLabel` 和 `JTextField`，并将它们添加到一个 `JPanel` 中：

```java
JPanel firstNamePanel = new JPanel();
firstNamePanel.add(new JLabel("First Name:"));
firstNamePanel.add(new JTextField(10));
```

默认情况下，`JPanel` 的内容从左到右排列。你也可以将 `JPanel` 设置为垂直显示内容。这里我们创建主 `JPanel`，将其布局设为垂直（Y 轴方向），并添加三个嵌套的 `JPanel`：

```java
JPanel mainPanel = new JPanel();
mainPanel.setLayout(new BoxLayout(mainPanel, BoxLayout.Y_AXIS));
mainPanel.add(firstNamePanel);
mainPanel.add(lastNamePanel);
mainPanel.add(buttonPanel);
```

界面不算很漂亮，但很简单，你可以通过嵌套 `JPanel` 来快速组织用户界面。

务必阅读并理解 [NestedPanelsExample.java](code/gui/NestedPanelsExample.java) 中的完整示例。

接下来，我们将了解用户交互的基本原理。

---

## 4.3 处理按钮点击

用户界面是**事件驱动**的：你为每种可能的事件指定要调用的方法。按钮点击就是一种事件。

<img src="images/ButtonClickExample.png" align="right" width="300px" style="margin:16px;"/>

每当用户事件发生时（按钮点击、在文本框中输入），Java 会创建一个_事件对象_，然后调用你指定的方法。你编写的方法叫做_事件监听器（event listener）_。

回想一下 `Submit` 按钮：

```java
JButton submit = new JButton("Submit");
```

这里，我们告诉按钮在被点击时调用 `actionPerformed` 方法：

```java
submit.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        String firstName = firstNameField.getText();
        String lastName = lastNameField.getText();
        JOptionPane.showMessageDialog(null, "Hello " + firstName + " " + lastName);
    }
});
```

按钮的方法总是叫做 `actionPerformed`。其他类型的组件有其他的监听器方法，稍后会遇到。

我们目前还没有足够的术语来完整解释这一点，但它实际上是在定义一个类！`new ActionListener() { … }` 部分创建了一个匿名（无名）类，其中包含一个方法 `actionPerformed`。

注意 `actionPerformed` 方法从 `firstNameField` 和 `lastNameField` 两个 `JTextField` 中获取文本。你可以访问封闭方法中声明的局部变量！这意味着其封闭作用域必须包含封闭方法的局部变量。这个机制既奇妙又有趣。

运行 `ButtonClickExample.java` 时，你会看到一个弹出窗口，这是由 `JOptionPane.showMessageDialog` 实现的。

务必阅读并理解 [ButtonClickExample.java](code/gui/ButtonClickExample.java) 中的示例。

### 4.3.1 练习：点击 `Cancel` 清空文本框

练习一下：为 `Cancel` 按钮添加一个事件监听器，通过调用 `setText` 方法清空文本框。首先需要重构 Cancel 按钮的代码：

旧代码：
```java
buttonPanel.add(new JButton("Cancel"));
```

新代码：
```java
JButton cancel = new JButton("Cancel");
buttonPanel.add(cancel);
```

现在你可以像代码对 `submit` 那样调用 `cancel.addActionListener`。

运行时，在姓名框中输入一些文字，然后点击 `Cancel`，文本框应该被清空。

## 延伸阅读

你可以访问以下链接，了解组织用户界面的不同方式：
https://docs.oracle.com/javase/tutorial/uiswing/layout/visual.html

以下链接提供了使用各种 Swing 组件的大量示例，如果你之后想要设计特定类型的界面，可以参考：
https://docs.oracle.com/javase/tutorial/uiswing/examples/components/index.html

建议使用嵌套 `JPanel` 配合 `BoxLayout`。这不是用户体验课程！我们不会对 UI 的美观程度评分！
