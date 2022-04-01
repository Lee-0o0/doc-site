# Java基础— —Object类详解

本文主要介绍Object类。

[toc]

## 一、源码

`Object`位于`java.lang`包下，下面是JDK 8中`Object`的源码：

```java
package java.lang;

/**
* Object类是所有类的根
*/
public class Object {

    private static native void registerNatives();
    static {
        registerNatives();
    }

    public final native Class<?> getClass();

    public native int hashCode();

    public boolean equals(Object obj) {
        return (this == obj);
    }

    protected native Object clone() throws CloneNotSupportedException;

    public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }

    public final native void notify();
    public final native void notifyAll();

    public final native void wait(long timeout) throws InterruptedException;
    
    public final void wait(long timeout, int nanos) throws InterruptedException {
        if (timeout < 0) {
            throw new IllegalArgumentException("timeout value is negative");
        }

        if (nanos < 0 || nanos > 999999) {
            throw new IllegalArgumentException(
                                "nanosecond timeout value out of range");
        }

        if (nanos > 0) {
            timeout++;
        }

        wait(timeout);
    }

    public final void wait() throws InterruptedException {
        wait(0);
    }

    protected void finalize() throws Throwable { }
}

```



## 二、方法详解

### 2.1 equals()

`equals()`方法用于比较两个对象是否相等，其默认实现如下：

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

在默认实现中，比较的是两个变量是否引用同一个对象。

在实际开发中，我们需要重写这个方法，进行值比较，比较常用的就是`String`的`equals()`方法。

如果要重写`equals()`方法，需要满足以下特性：

- 自反性：`x.equals(x)`的值为`true`；

- 对称性：即`x.equals(y)`与`y.equals(x)`的结果相同；
- 传递性：若`x.equals(y)`和`y.equals(z)`为`true`，则`x.equals(z)`结果也为`true`；
- 一致性：`x.equals(y)`结果一直一样；
- 对于任意的非空引用值x，`x.equals(null)`必须返回`false`；

现给出重写`equals()`方法的步骤：

1. 比较this与obj是否引用同一个对象：

   ```java
   if(this == obj) return true;
   ```

2. 判断obj是否为空：

   ```java
   if(obj == null) return false;
   ```

3. 比较this与obj是否属于同一个类，这分为两种情况：

   - 如果`equals()`的语义在所有的子类中拥有统一的语义，使用`instanceof`：

     ```java
     if( !(obj instanceof ClassName) ) return false; 
     ```

   - 如果`equals()`的语义在子类中有所改变，则使用`getClass()`：

     ```java
     if(getClass() != obj.getClass() ) return false;
     ```

4. 将obj进行类型转换，对相应的属性进行比较。基本类型用`==`，引用类型用`equals()`：

   ```java
   ClassName other = (ClassName)obj;
   return field1 == other.field1 && field2.equals(other.field2) &&...;
   ```



### 2.2 hashCode()



如果重写了`equals()`方法，则要重写`hashCode()`方法，否则