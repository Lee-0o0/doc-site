# Java进阶— —源码：List

本文主要介绍JDK 1.8版本下`List`实现类的源码。

[toc]

## 1. ArrayList

本章主要讲解`ArrayList`的源码。首先来看`ArrayList`的声明：

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```

- `ArrayList`支持泛型，继承了抽象类`AbstractList`，不用自己全部实现通用方法；
- `ArrayList`实现了接口`List`；
- `ArrayList`实现了接口`RandomAccess`，表示可以随机访问；`RandomAccess`只是一个标识接口，没有方法；
- `ArrayList`实现了接口`Cloneable`，表示可以克隆；
- `ArrayList`实现了接口`Serializable`，表示可以序列化；



### 1.1 属性和构造方法

首先来看`ArrayList`的属性：

```java
// 静态属性
// 默认数组长度
private static final int DEFAULT_CAPACITY = 10;
// 空数组
private static final Object[] EMPTY_ELEMENTDATA = {};
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
// 数组最大长度
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

// 非静态属性
// 存放元素的数组
transient Object[] elementData;
// 实际存放在数组中的元素数量
private int size;
```

**注意：**

虽然`ArrayList`支持泛型，但是底层存放元素的数组声明为了`Object[]`。

为什么空数组有两个呢？https://blog.csdn.net/GUOCHUC/article/details/107324793

构造方法：

```java
// 无参构造方法
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
// 指定初始容量的构造方法
public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) {
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        throw new IllegalArgumentException("Illegal Capacity: " + initialCapacity);
    }
}
// 根据集合初始化ArrayList
public ArrayList(Collection<? extends E> c) {
    elementData = c.toArray();
    size = elementData.length
    if (size != 0) {
        // 集合c中有元素，这个判断是干什么的呢？
        if (elementData.getClass() != Object[].class)
            elementData = Arrays.copyOf(elementData, size, Object[].class);
    } else {
        // 集合c中没有元素，数组初始化为空数组
        this.elementData = EMPTY_ELEMENTDATA;
    }
}
```

如果集合c中有元素，那么直接通过`elementData = c.toArray()`就可以获取到集合中的元素，那么嵌套的`if`是干什么的呢？源代码中有以下注释：

```java
// defend against c.toArray (incorrectly) not returning Object[]
// (see e.g. https://bugs.openjdk.java.net/browse/JDK-6260652)
if (elementData.getClass() != Object[].class)
    elementData = Arrays.copyOf(elementData, size, Object[].class);
```

这是一个官方bug，关于这个bug的解释是这样的：

[1] https://www.cnblogs.com/liqing-weikeyuan/p/7922306.html

[2] https://blog.csdn.net/GuLu_GuLu_jp/article/details/51457492

简单解释就是某个集合类重写了`toArray()`方法，这个方法返回值不是`Object[]`，而是某个具体的类，那么后续存储时可能会导致类型转换出错。



### 1.2 get(int index)方法

`get(int index)`方法可以获取指定索引的元素。

```java
public E get(int index) {
    Objects.checkIndex(index, size);
    return elementData(index);
}

E elementData(int index) {
    return (E) elementData[index];
}
```



### 1.3 set(int index, E element)方法

`set(int index,E element)`方法可以将指定索引位置的元素修改为新的元素值，并将旧元素值返回。

```java
public E set(int index, E element) {
    rangeCheck(index);

    E oldValue = elementData(index);
    elementData[index] = element;
    return oldValue;
}
```



### 1.4 add(E e)方法

`add(E e)`方法可以在数组末尾添加一个元素，如果数组满了，涉及到扩容操作。

```java
public boolean add(E e) {
    ensureCapacityInternal(size + 1);  
    elementData[size++] = e;
    return true;
}
```

方法`ensureCapacityInternal(int minCapacity)`原码如下：

```java
private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

// 计算新数组的最小容量
private static int calculateCapacity(Object[] elementData, int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity;
}

// 扩容操作
private void ensureExplicitCapacity(int minCapacity) {
    modCount++;

    // 如果新数组的最小容量比当前数组的容量大，则执行扩容操作
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}
```

真正的扩容方法`grow(int minCapacity)`源码如下：

```java
private void grow(int minCapacity) {
    int oldCapacity = elementData.length;        // 旧数组长度
    int newCapacity = oldCapacity + (oldCapacity >> 1);   // 新数组的长度为旧长度1.5倍
    
    // 如果计算的新数组长度比希望的最低长度小，则按照希望的最低容量进行扩容 
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    
    // MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
    // 如果新的数组长度比最大的数组长度，则将其赋值为MAX_ARRAY_SIZE或Integer.MAX_VALUE
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    
    // 扩容操作
    elementData = Arrays.copyOf(elementData, newCapacity);
}

private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) 
        throw new OutOfMemoryError();
    return (minCapacity > MAX_ARRAY_SIZE) ? Integer.MAX_VALUE : MAX_ARRAY_SIZE;
}
```

除了在数组末尾增加元素外，还可以在指定索引位置插入元素：

```java
public void add(int index, E element) {
    rangeCheckForAdd(index);

    ensureCapacityInternal(size + 1);  
    // 将index后面的元素后移
    System.arraycopy(elementData, index, elementData, index + 1,size - index);
    elementData[index] = element;
    size++;
}
```



### 1. 5 remove(int index)方法

`remove(int index)`可以将指定索引位置的元素去除，并返回该元素：

```java
public E remove(int index) {
    rangeCheck(index);

    modCount++;
    E oldValue = elementData(index);

    // index后面的元素往前面移动
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,numMoved);
    
    // 将最后一个元素置为null
    elementData[--size] = null; 

    return oldValue;
}
```

除了可以删除指定索引位置的元素，还可以将指定元素删除`remove(Object o)`，返回操作结果。主要思路就是遍历数组，找到要删除的元素的下标，然后调用`remove(int index)`方法（只不过此处改用了`fastRemove(index)`方法）：

```java
public boolean remove(Object o) {
    if (o == null) {
        for (int index = 0; index < size; index++)
            if (elementData[index] == null) {
                fastRemove(index);
                return true;
            }
    } else {
        for (int index = 0; index < size; index++)
            if (o.equals(elementData[index])) {
                fastRemove(index);
                return true;
            }
    }
    return false;
}

private void fastRemove(int index) {
    modCount++;
    
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,numMoved);
    
    elementData[--size] = null; 
}
```



## 2. LinkedList

首先来看`LinkedList`的声明:

```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable


public abstract class AbstractSequentialList<E> extends AbstractList<E> 
```

- 继承了抽象类`AbstractSequentialList`，由于`LinkedList`底层是链表结构，所以没有直接继承`AbstractList`抽象类；
- 实现了接口`List`；
- 实现了双端队列接口`Deque`，可以将`LinkedList`作为队列使用；
- 实现了接口`Cloneable`，可以克隆；
- 实现了接口`Serializable`，可以序列化；



### 2.1 属性和构造方法

首先来看属性：

```java
transient int size = 0;         // 元素个数
transient Node<E> first;        // 链表头指针
transient Node<E> last;         // 链表尾指针

// 由于LinkedList是基于链表的，所以节点声明为Node，是一个双端链表
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

构造方法如下：

```java
public LinkedList() {
}
public LinkedList(Collection<? extends E> c) {
    this();
    addAll(c);
}
```

构造方法只有两个，无参构造方法和指定集合的构造方法，没办法指定初始容量（`LinkedList`是链表，指定初始容量也没有意义）。



### 2.2 get(int index)方法

`get(int index)`可以获取指定索引的元素：

```java
public E get(int index) {
    checkElementIndex(index);
    return node(index).item;
}

Node<E> node(int index) {
    // index 在前半部分，从头指针开始遍历
    if (index < (size >> 1)) {
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
    // index 在后半部分，从尾指针开始遍历
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```



### 2.3 set(int index,E e)方法

`set(int index,E e)`可以更改指定位置的元素，并返回旧元素：

```java
public E set(int index, E element) {
    checkElementIndex(index);
    // 找到指定位置的节点
    Node<E> x = node(index);
    // 获取旧元素的值
    E oldVal = x.item;
    // 更新
    x.item = element;
    return oldVal;
}
```



### 2.4 add(E e)方法

`add(E e)`方法在链表末尾添加一个元素：

```java
public boolean add(E e) {
    linkLast(e);
    return true;
}

void linkLast(E e) {
    final Node<E> l = last;
    final Node<E> newNode = new Node<>(l, e, null);
    last = newNode;
    if (l == null)
        first = newNode;
    else
        l.next = newNode;
    size++;
    modCount++;
}
```



### 2.5 remove(int index)方法

`remove(int index)`可以将指定索引位置的元素删除：

```java
public E remove(int index) {
    checkElementIndex(index);
    // node(index)找到指定位置的元素，然后unlink(node)删除指定元素，返回删除的元素
    return unlink(node(index));
}

E unlink(Node<E> x) {
    final E element = x.item;
    final Node<E> next = x.next;
    final Node<E> prev = x.prev;

    if (prev == null) {      // 删除的是第一个元素
        first = next;
    } else {                 // 删除的不是第一个元素
        prev.next = next;
        x.prev = null;
    }

    if (next == null) {      // 删除的是最后一个元素
        last = prev;
    } else {                 // 删除的不是最后一个元素
        next.prev = prev;
        x.next = null;
    }

    x.item = null;           // 断开引用，方便垃圾回收
    size--;
    modCount++;
    return element;
}
```



## 3. 关于链表序列化问题

在`ArrayList`中，存放元素的数组声明为`transient`，表示序列化时该数组并不会序列化：

```java
transient Object[] elementData;
```

为什么呢？我们应该明白，序列化时，该数组可能只有少量的元素，如果将整个数组都序列化，那么会造成空间浪费。所以在`ArrayList`中重写了方法`writeObject()`和`readObject()`，用于序列化和反序列。当对象输出出入流序列化列表时，就会调用列表中的方法。

`writeObject()`的源码：

```java
private void writeObject(java.io.ObjectOutputStream s) throws java.io.IOException{
    // Write out element count, and any hidden stuff
    int expectedModCount = modCount;
    s.defaultWriteObject();

    // 写出数组中的元素数量
    s.writeInt(size);

    // 写出数组中的每个元素，而不是写出整个数组
    for (int i=0; i<size; i++) {
        s.writeObject(elementData[i]);
    }

    // 如果在序列化过程中，有其他线程修改了数组结构（例如插入、删除），则抛出异常
    if (modCount != expectedModCount) {
        throw new ConcurrentModificationException();
    }
}
```

`readObject()`方法：

```java
private void readObject(java.io.ObjectInputStream s)
    throws java.io.IOException, ClassNotFoundException {
    elementData = EMPTY_ELEMENTDATA;

    s.defaultReadObject();

    // Read in capacity
    s.readInt(); 

    if (size > 0) {
        // be like clone(), allocate array based upon size not capacity
        int capacity = calculateCapacity(elementData, size);
        SharedSecrets.getJavaOISAccess().checkArray(s, Object[].class, capacity);
        ensureCapacityInternal(size);

        Object[] a = elementData;
        // Read in all elements in the proper order.
        for (int i=0; i<size; i++) {
            a[i] = s.readObject();
        }
    }
}
```

