# UML— —UML类图

本文主要介绍UML类图。


## 一、UML图表示类

在UML中，类使用包含类名、属性和方法且带有分隔线的长方形来表示。如定义一个`Employee`类，它包含属性`name`、`age`和`email`，以及操作`modifyInfo()`，UML类图如下：

![image-20200817154910650](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/d2c20ecbd4d0df5e55a1ae328fb04efc--ee7c--image-20200817154910650.png)

其Java代码实现如下：

```java
public class Employee {
	private String name;
	private int age;
	private String email="缺失";
	
	public void modifyInfo() {
		......
	}
}
```



在UML类图中，类一般由三部分组成：

**(1)** 第一部分是类名：每个类都必须有一个名字，类名是一个字符串。

**(2)** 第二部分是类的属性(Attributes)：即类的成员变量。一个类可以有任意多个属性，也可以没有属性

UML规定属性的表示方式为：

<p align="center" style="font:15;color:red">可见性 名称 : 类型 [ = 缺省值 ]</p>

其中：

- “可见性”表示该属性对于类外的元素而言是否可见，包括公有(public)、私有(private)和受保护(protected)三种，在类图中分别用符号`+`、`-`和`#`表示。
- “名称”表示属性名，用一个字符串表示。
- “类型”表示属性的数据类型，可以是基本数据类型，也可以是用户自定义类型。
- “缺省值”是一个可选项，即属性的初始值。

**(3)** 第三部分是类的操作(Operations)：操作是类的任意一个实例对象都可以使用的行为，是类的成员方法。

UML规定操作的表示方式为：

<p align="center" style="font:15;color:red">可见性 名称 (参数列表)  [ : 返回类型]</p>

其中：

- “可见性”的定义与属性的可见性定义相同。
- “名称”即方法名，用一个字符串表示。
- “参数列表”表示方法的参数，其语法与属性的定义相似，参数个数是任意的，多个参数之间用逗号`,`隔开。
- “返回类型”是一个可选项，表示方法的返回值类型，依赖于具体的编程语言，可以是基本数据类型，也可以是用户自定义类型，还可以是空类型(void)，如果是构造方法，则无返回类型。



## 二、UML图表示类的关系

我们可以通过UML图来表示各个类之间的关系。

### 2.1 泛化关系

泛化(Generalization)关系也就是继承关系，用于描述父类与子类之间的关系，父类又称作基类或超类，子类又称作派生类。**==在UML中，泛化关系用带空心三角形的直线来表示==**。

例如，现有一个`Person`类，一个`Student`类继承自`Person`类：

```java
class Person{
    ...
}

class Student extends Person{
    ...
}
```

其UML类图表示如下：

![image-20200817160257975](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/72ded489587863f6a919ec35c9fbed17--b1e8--image-20200817160257975.png)



### 2.2 实现关系

UML中用与类的表示法类似的方式表示接口，只不过没有属性，例如：

![image-20200817160525076](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/6bc44630b61c7f8c1946ddfce4411d00--7333--image-20200817160525076.png)

**==在UML中，类与接口之间的实现关系用带空心三角形的虚线来表示。==**

例如，现在有一个接口`IEmployee`，类`Employee`实现了该接口：

```java
interface IEmplyee{
    ...
}

class Employee implements IEmployee{
    ...
}
```



![image-20200817160659161](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/d7bd20c4a9b22799544181123072b059--0086--image-20200817160659161.png)



### 2.3 依赖关系

依赖(Dependency)关系是一种使用关系，在需要表示一个事物使用另一个事物时使用依赖关系。

依赖关系通常通过三种方式来实现，第一种也是最常用的方式是将一个类的对象作为另一个类中方法的参数，第二种方式是在一个类的方法中将另一个类的对象作为其局部变量，第三种方式是在一个类的方法中调用另一个类的静态方法。

**==在UML中，依赖关系用带箭头的虚线表示，由依赖的一方指向被依赖的一方。==**

例如，现在有一个司机类`Driver`，其有一个方法`drive(Car car)`，其参数需要用到汽车类`Car`对象：

```java
public class Driver {
	public void drive(Car car) {
        ...
	}
}

public class Car {
    ...
}  
```

则UML图如下：

![image-20200817161357367](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/4cdce21cf53bd7893b001e5768d9065d--dab7--image-20200817161357367.png)





### 2.4 关联关系

关联(Association)关系是类与类之间最常用的一种关系，它是一种结构化关系，用于表示一类对象与另一类对象之间有联系，如汽车和轮胎、师傅和徒弟、班级和学生等等。**==在UML类图中，用实线连接有关联关系的对象所对应的类，通常将一个类的对象作为另一个类的成员变量。==**在使用类图表示关联关系时可以在关联线上标注角色名，一般使用一个表示两者之间关系的动词或者名词表示角色名（有时该名词为实例对象名），关系的两端代表两种不同的角色，因此在一个关联关系中可以包含两个角色名，角色名不是必须的，可以根据需要增加，其目的是使类之间的关系更加明确。

#### 2.4.1 单向关联

**单向关联用带箭头的实线表示**，即有一个类的成员变量为另一个类的对象。

例如，一个顾客`Customer`有一个地址`Address`:

```java
class Customer {
	private Address address;
	……
}

public class Address {
	……
}
```

![image-20200817214110924](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/a0f3027e4b0fc844cdbf9a9cb17ecbe4--e9ef--image-20200817214110924.png)



#### 2.4.2 双向关联

在有的情况下，两个类拥有另一个类的成员变量，这样可以用双向关联来表示，**双向关联用带双箭头的实现表示**。例如，一个老师`Teacher`可以教授课程`Subject`，一个课程`Subject`被一个老师教授：

```java
class Teacher{
    private Subject[] subjects;
    ...
}

class Subjetc{
    private Teacher teacher;
    ...
}
```

![image-20200817214612209](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/2c8f635f1855e2ff9a879310d4f5d280--408d--image-20200817214612209.png)



#### 2.4.3 自关联

在系统中可能会存在一些类的属性对象类型为该类本身，这种特殊的关联关系称为自关联。

例如，在链表中，一个节点`Node`的后继节点同为节点`Node`:

```java
class Node{
    private Node next;
    ...
}
```

![image-20200817214836080](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/1ee92fd360145fc4054d8f2885507d4f--f338--image-20200817214836080.png)





#### 2.4.4 聚合关系

聚合(Aggregation)关系表示整体与部分的关系。在聚合关系中，成员对象是整体对象的一部分，但是成员对象可以脱离整体对象独立存在。**==在UML中，聚合关系用带空心菱形以及箭头的直线表示，并且空心菱形一端为整体，箭头的一端为部分==**。

例如，汽车`Car`拥有引擎`Engine`:

```java
class Car {
	private Engine engine;
	......
}

public class Engine {
	……
} 
```

![image-20200817220806826](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/2cc2784997104317da15d2421edca1ea--cf9e--image-20200817220806826.png)



#### 2.4.5 组合关系

组合(Composition)关系也表示类之间整体和部分的关系，但是在组合关系中整体对象可以控制成员对象的生命周期，一旦整体对象不存在，成员对象也将不存在，成员对象与整体对象之间具有同生共死的关系。**==在UML中，组合关系用带实心菱形和箭头的直线表示，并且实心菱形一端为整体，箭头的一端为部分。==**

例如，一个人`Person`有头`Head`:

```java
class Person{
    private Head head;
    ...
}

class Head{
    ...
}
```

![image-20200817221233117](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/4b66d3b6c386af6aaa828c82d2104c8f--bda6--image-20200817221233117.png)



#### 2.4.6 多重性关联

多重性关联关系，表示两个关联对象在数量上的对应关系。**==在UML中，对象之间的多重性可以直接在关联直线上用一个数字或一个数字范围表示==**。

![image-20200817222127796](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/e470316358973d93b81e9399c25ab155--7ae2--image-20200817222127796.png)

例如：一个界面`Form`可以拥有零个或多个按钮`Button`，但是一个按钮只能属于一个界面:

```java
class Form{
    private Button[] buttons;
    ......
}

class Button{
    private Form form;
    ......
}
```

![image-20200817222446083](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/d7d7af0b061d4f4857ea697990c404fb--4596--image-20200817222446083.png)