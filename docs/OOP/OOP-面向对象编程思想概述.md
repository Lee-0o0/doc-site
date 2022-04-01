# OOP— —面向对象编程思想概述

本文主要以Java编程语言为例，介绍面向对象编程思想以及类与对象的概念。



## 1. 什么是面向对象编程

面向对象编程（Object Oriented Programming，简称OOP），是一种程序设计思想。OOP把对象作为程序的基本单元，一个对象包含了数据和操作数据的函数。对象的模板称为类。

我们以现实生活中的例子来说明面向对象编程中的类与对象的概念：

![image-20220307201533545](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-03-07/66df588cd3d09dfcbe86c0b94e11a479--4429--image-20220307201533545.png)

<center><span ><font color="gray">图1：犬的分类图</font></span></center>

动物—犬有很多分类，比如拉布拉多犬、柴犬、阿拉斯加犬等，而上图的小黑、小白、小黄是具体的一只犬，他们分属于某一种犬。

上面的图就已经包含了类与对象的概念，犬、拉布拉多犬和柴犬是类，小黑、小白和小黄是对象。

类是一个模板，它描述一类对象的行为和状态。

对象是类的一个实例，有状态和行为。



## 2. 类与对象的使用

Java是一门面向对象编程语言，我们以Java为例，讲解类与对象的基本概念。

### 2.1 类的声明

在Java中，类的声明语法如下：

```java
[类修饰符] class 类名{
    属性列表;
    方法列表;
}
```

在类中，声明一个属性的语法如下：

```java
[属性修饰符] 数据类型 属性名;
```

声明方法的语法如下：

```java
[方法修饰符] 方法返回值数据类型 方法名(参数列表);
```

例如，我们可以定义一个类`Dog`：

```java
public class Dog {
    public String name;
    public String gender;
    public Date birthday;

    public void bark(){
        System.out.println("汪汪汪...");
    }
}
```

类`Dog`包含三个属性：名字`name`、性别`gender`和出生日期`birthday`，以及一个方法`bark()`。



### 2.2 对象的创建

当定义了类之后，只是定义了一个模板，就像我们有了施工图纸，但并没有高楼大厦。但是，我们可以根据施工图纸建设高楼，我们也可以根据类创建对象。

对象的创建使用关键字`new`：

```java
类名 对象名 = new 类名();
```

例如：

```java
Dog littleBlackDog = new Dog();
```

这样，我们就创建了一个对象`littleBlackDog`，它的类是`Dog`，说明它是犬。这样，对象`littleBlackDog`就有了类中定义的属性和方法，我们可以使用点运算符`.`来访问属性和方法：

```java
littleBlackDog.name = "小黑";
littleBlackDog.gender = "公";
littleBlackDog.birthday = new Date(2010,1,1);

littleBlackDog.bark();           // 输出 汪汪汪...
```

其实，类与C语言中的结构体类型，只不过类中多了方法（函数）。



## 3. 类成员与对象成员

在上一节中，我们通过`对象名.成员名`方式来访问属性或调用方法，这样的属性或方法称为对象成员。

本节我们将学习类成员和对象成员。

- 类成员：类成员属于类，不属于一个具体的对象，我们可以通过`类名.成员名`来访问类成员；
- 对象成员：对象成员属于一个具体的对象，我们可以通过`对象名.成员名`来访问对象成员；

那么我们要如何区分类成员和对象成员呢？很简单，只需要在成员（属性或方法）声明前加上关键字`static`，就可以使其变为类成员。例如：

```java
public class Dog {
    public static long NUMBER;        // 类成员属性
    public static void count(){       // 类成员方法
        System.out.println("狗的总数量为："+NUMBER); 
    }
    
    public String name;
    public String gender;
    public Date birthday;

    public void bark(){
        System.out.println("汪汪汪...");
    }
}
```

我们在之前的例子中，加入了类成员属性和类成员方法，使用类成员如下：

```java
Dog.NUMBER = 1000;
Dog.count();
```

需要注意的是，我们不能在类成员方法中使用对象成员，因为并不能对象成员存在于对象中，而对象并不一定会在程序中创建，下面的做法是错误的：

```java
public class Dog {
    public static long NUMBER;        
    public static void count(){       
        System.out.println("name="+name);    // 错误：在类成员方法中使用对象成员属性
        bark();                              // 错误：在类成员方法中调用对象成员方法
        System.out.println("狗的总数量为："+NUMBER); 
    }
    
    public String name;
    public String gender;
    public Date birthday;

    public void bark(){
        System.out.println("汪汪汪...");
    }
}
```



## 4. OOP三特性

面向对象编程三特性为

- 封装：类和对象中的成员是否对外暴露；
- 继承：子类可以继承父类的属性和方法；
- 多态：多态是指同一个行为具有不同表现的现象；

将在后面的博文中逐一讲解。
