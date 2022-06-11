# 一个多线程问题：顺序打印ABCD

[toc]



## 1. 问题描述

现有四个线程【Thread-1，Thread-2，Thread-3，Thread-4】，线程1打印A，线程2打印B，线程3打印C，线程4打印D，并且按照ABCD的顺序打印n次。



## 2. 解决方法

### 2.1 方法一

利用`synchronized`,`wait()`和`notifyAll()`解决，由于唤醒线程时无法指定唤醒，因此需要循环判断。

```java

```

