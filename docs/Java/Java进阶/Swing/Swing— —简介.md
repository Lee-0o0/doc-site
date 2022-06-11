# Swing— —简介

本文介绍什么是Swing以及Swing与AWT的区别

[toc]

## 一、什么是Swing

Swing 是一个用于 Java GUI 编程（图形界面设计）的工具包（类库）；换句话说，Java 可以用来开发带界面的 PC 软件，使用到的工具就是 Swing。

Swing 使用纯粹的 Java 代码来模拟各种控件（使用 Java 自带的作图函数绘制出各种控件），没有使用本地操作系统的内在方法，所以 Swing 是跨平台的。也正是因为 Swing 的这种特性，人们通常把 Swing 控件称为轻量级控件。



## 二、Swing与AWT

AWT（Abstract Window Toolkit，抽象窗口工具）是一套早期的 Java GUI 开发工具，Swing 也是在 AWT 的基础上发展起来的。



![Swing是在AWT的基础上发展起来的](img/Swing— —简介/1-1Q105123211B2.gif)


AWT 的初衷是用来开发小型的图形界面程序，提供的功能较少，诸如剪切板、打印支持、键盘导航、弹出式菜单、滚动窗格等很多重要的功能在 AWT 中都不具备；此外，AWT 发生错误的几率也很高。

Java 官方看到了 AWT 的不足，就开始着手开发新的 GUI 类库，以继续占领 Java GUI 开发的市场，这就是后来的 Swing。

Swing 弥补了 AWT 的不足，并对 AWT 进行了扩充，几乎支持了所有的常用控件和功能，它们不但更加漂亮，而且更加易用，真正实现了“一次编译，到处运行”的承诺。

目前，Swing 已经代替 AWT 成为 Java 图形界面设计的首选，相对于 AWT 来说，Swing 有过之而无不及。