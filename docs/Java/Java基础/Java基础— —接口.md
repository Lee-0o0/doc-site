# Java基础— —接口

本文主要介绍接口的知识。

[toc]

## 一、接口的概念

**接口定义了一组规范。**

更通俗地说，接口定义了一组方法，但没有实现；实现接口的类，必须实现接口中定义的方法。



## 二、接口的使用

### 2.1 定义自己的接口

我们可以使用以下语法定义接口:

```java
[访问修饰符] interface 接口名 extends 父接口1, 父接口2...{
 	// 静态常量
    // 抽象方法
}
```

关键点：

- **接口可以多继承**，使用`extends`继承父接口中声明的方法；
- 接口中可以声明常量，默认为`public static final`；
- 接口中声明的方法都是抽象的，默认为`public abstract`；



### 2.2 实现接口

如果一个类要实现一个接口，可以使用关键字`implements`，类可以实现多个接口。

```java
[访问修饰符] class 类名 implements 接口1, 接口2...{
    // 实现接口中定义的方法
}
```

注意：

- 在实现接口的方法时，必须把方法声明为`public`；这是因为子类重写方法的访问权限不能比父类中被重写的方法的访问权限更低。



## 三、接口的改变

在Java 8中，接口有了重大改变。在Java 8及以后，我们可以在接口中定义默认方法和静态方法，也就是说，接口中的方法可以有方法体了。

### 3.3.1 默认方法

我们可以为接口方法提供一个默认实现，**必须用`default`修饰符标记默认方法**。

```java
[访问修饰符] interface 接口名{
    // 默认方法
    default 返回类型 方法名(参数列表){
        方法体
    }
}
```

默认方法可以调用其它接口方法。

为什么接口要向抽象类看齐，可以有默认方法呢？

现假设一个情景，一个类`Bag`实现了接口`Collection`。后来，接口增加了新方法，那么实现了该接口的类都需要实现新方法，如果我们增加的是默认方法，那么类就不用实现新方法也能正常运行，保证了”源代码兼容“。



**解决默认方法冲突**

如果先在一个接口中将一个方法定义为默认方法，然后又在父类或另一个接口中定义同样的方法，Java要如何解决呢？

1. **父类优先**。如果父类提供了一个具体方法，接口中同名而且有相同参数类型的默认方法会被忽略。

   ```java
   // 接口
   public interface Named {
       default void printName(String name){
           System.out.println("我是接口");
           System.out.println(name);
       }
   }
   // 父类
   public class Person {
       public void printName(String name){
           System.out.println("我是父类");
           System.out.println(name);
       }
   }
   // 子类，继承父类，实现接口
   public class Child extends Person implements Named{
       public static void main(String[] args) {
           Child child = new Child();
           child.printName("张三");
       }
   }
   ```

   结果：

   ```txt
   我是父类
   张三
   ```

   

2. **接口冲突。**如果一个接口提供了一个默认方法，另一个接口提供了一个同名且参数相同的方法（不论是否是默认方法），实现类必须覆盖这个方法来解决冲突。

   ```java
   // 接口一
   public interface Named {
       default String getName(){
           return "Named";
       }
   }
   // 接口二
   public interface Person {
       default String getName(){
           return "Person";
       }
   }
   // 实现类
   public class Student  implements Person , Named{
   }
   ```

   结果编译器在实现类上报错：

   ![image-20210222111058879](img/Java基础— —接口/image-20210222111058879.png)

   我们必须重新实现冲突的方法，选择两个冲突方法中的一个是一种解决办法：

   ```java
   public class Student implements Person , Named{
       @Override
       public String getName() {
           return Person.super.getName();
       }
   }
   ```



### 3.3.2 静态方法

在java 8中，允许在接口中增加静态方法，语法为：

```java
[访问修饰符] interface 接口名 {
    public static 返回类型 方法名(参数列表){
        方法体
    }
}
```

我们可以直接通过`接口名.静态方法名(参数)`调用静态方法。

如果实现类也有相同的静态方法，与接口中的静态方法不冲突，两者分别从属于接口和类，我们可以通过`类名.静态方法名(参数)`进行调用实现类的静态方法。



### 3.3.3 默认方法与静态方法

默认方法可以调用其它方法（静态方法和抽象方法），静态方法只能调用其它静态方法，不能调用非静态方法。