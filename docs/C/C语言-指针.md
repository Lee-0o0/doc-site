# C语言— —指针

本文介绍C语言中关于指针的基础知识。

## 1. 什么是指针？

在说明什么是指针之前，我们可以先了解一下内存。我们可以把内存视为一长串小格子，每个格子为一个字节。从左到右编号，作为该字节的地址：

![image-20210922205823286](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-02-25/966d61e764231dbda8b6d67a2a873bbe--05e6--image-20210922205823286.png)

为了存储更大的值，我们将多个字节合在一起作为一个更大的内存单位，如一个int数组，每个元素占4个字节：

![image-20210922210003531](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-02-25/d82cfc1ed7e9c9b4b0aec6ef12487ee4--86e3--image-20210922210003531.png)

那么每个元素的地址是多少呢？比如第一个元素，它占据了第0个到第3个字节，总共四个字节，那么该元素的地址有四个吗？不是的，该元素只有一个地址，即不论一个元素包含了多少字节，只有一个字节。至于它的地址是它最左边那个字节的位置，还是最右边那个字节的位置，不同的机器有不同的规定。

说完地址，那么我们就可以继续说说字节中的内容了：

![image-20210922210702286](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-02-25/88c4bc73779f3ab9fc3afc9b64f0363a--7ef0--image-20210922210702286.png)

上图有5个元素，每个元素占据4个字节的大小。小格子里面的内容就是存储在其中的数据，第一个格子存储了整数1000，第二个格子存储了负数-1，第三个格子存储了107852331，第四个格子存储了100，第五个格子存储了108。我们可以根据这些格子的地址来访问格子里的内容，就像根据门牌号（房屋地址）来找到房屋进而发现房屋里的东西一样。但是，要记住这些地址实在是太难了，所以高级语言的特性之一就是通过名字而不是地址来访问内存的位置。如下图，使用名字代替地址：

![image-20210922212114557](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-02-25/c88c6fbd7c02e2c6a9e53704b0a8b79d--4760--image-20210922212114557.png)

名字与内存地址之间的关联是由编译器为我们实现的，硬件仍然是通过地址来访问内存的。

现在我们来看看上述的5个元素到底是什么：

```c
int a = 1000;
int b = -1;
float c = 3.14;
int *d = &a;
float *e = &c;
```

a、b、c三个变量是我们熟悉的，那么d和e是什么呢？d和e就是我们本文要讲解的指针。**什么是指针？你可以把指针理解为地址。如果一个变量被声明为指针类型，那么该变量存储的内容是一个地址值。**

比如，d存储的值是变量a的地址，即100；变量e存储的是变量c的地址，即108。**`&` 称为取地址符，表示获取操作数在内存中的地址值。**

**注意：**5个变量在内存中的位置可能并不是挨在一起的，此处为了方便画在了一起。



## 2. 指针的声明与初始化

在C语言中，指针声明与初始化方式如下：

```c
数据类型 *指针[, *指针2......] [ = 地址];
```

- 数据类型表示 指针存储的地址值 所指向的内存位置 存储的数据是什么类型的；
- 我们可以在一行中声明多个指针，注意每个指针前面都要有`*`号；

例如在上面的例子中：

```c
int *d = &a;
```

表示指针d所存储的地址值100，指向内存地址为100的位置，该位置（100）存储一个 int 型数据；

```c
float *e = &c;
```

表示指针e所存储的地址值108，指向内存地址为108的位置，该位置（108）存储一个 float 型数据；

我们可以使用`&`来获取操作数在内存中的地址值，并将其赋值给指针。我们也可以给指针赋值为NULL，表示该指针不指向任何东西。

```c
int *d = NULL;
```



## 3. 取值运算符

在了解了指针的意义后，上述5个变量的示意图可以修改如下：

![image-20210922215818668](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-02-25/3b996aa6568f92f2e7d85764d25f79b4--84ed--image-20210922215818668.png)

其中指针d 中的箭头表示其中存储的地址为 a 的地址；指针e中的箭头表示其中存储的地址为 c 的地址。

那么我们可否根据指针d获取到变量a的值呢？当然可以，你知道了地址，怎么会找不到存储在该地址的内容呢。我们可以**使用取值运算符 `*`来访问指针指向的地址所存储的内容。**如下：

```c
int a = 10;
int *d = &a;
printf("a = %d\n*d = %d\n",a,*d);
```

```txt
a = 10
*d = 10
```

取值运算符是单目运算符，后接指针名，表示获取指针指向的地址所存储的内容。

那么我们也可以通过指针来修改指针指向的地址所存储的内容：

```c
int a = 10;
int *d = &a;
*d = 99;           // 通过指针d修改变量a的值 
printf("a = %d\n*d = %d\n",a,*d);
```

```txt
a = 99
*d = 99
```

如何获取地址值呢？在格式化输出中，我们可以通过修饰符`%p`来输出地址值：

```c
int a = 10;
int *d = &a;
printf("a的地址：%p\nd的值：%p",&a,d); 
```

```txt
a的地址：000000000062FE14
d的值：000000000062FE14
```

可见d存储的值就是变量a的地址。



## 4. 指针运算

### 4.1 自增运算符

指针可以使用自增运算符，但自增运算符和指针的顺序不同，结果也不同。如：

```c
int a = 10;              // 1. 声明整型变量a并赋值为10
int *p = $a;             // 2. 声明指针变量p，存储的内容为变量a的地址
int *p1 = p++;           // 3. 声明指针变量p1
```

如果a的地址为x，那么经过第2步后p的值也为x，在第3步后，p1的值为x，p的值为x+4：



![image-20210923210259503](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-02-25/43505869ecda6d36f3b3eb3b80f89756--70f1--image-20210923210259503.png)

```c
int a = 10;       // 1. 声明整型变量a并赋值为10
int *p = $a;      // 2. 声明指针变量p，存储的内容为变量a的地址
int *p1 = ++p;    // 3. 声明指针变量p1
```

如果a的地址为x，那么经过第2步后p的值也为x，在第3步后，p1的值为x+4，p的值为x+4：

![image-20210923210542748](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-02-25/441c5e66806b003877a3801a91b43d2d--741d--image-20210923210542748.png)

自减运算符同理。



### 4.2 +和-运算符

指针也可以使用 +（加） 和 -（减）运算符，不过只有以下两种情况：

- 指针 ± 整数
- 指针 - 指针

并且，只有指针指向数组时，指针和加减运算符的使用才有意义。

**指针 ± 整数**

指针 ± 整数 的结果是指针。例子：

```c
int nums[] = {1,2,3,4,5,6,7,8,9,10};
int *p = nums;
int *p1 = p + 2;
```

注意，数组名就是该数组的地址，也是首个元素的地址。所以 `p = nums` 和 `p = &nums[0]` 相同。

结果指针p1指向nums[2]元素所在的地址，即p1的值为 &nums[2]。即如果一个指针指向数组中的某个元素，则该指针加减某一个整数，则结果为指针，并且结果指针指向与该元素相离该整数位的元素。



**指针 - 指针**

指针 - 指针 的结果是整数。例子：

```c
int nums[] = {1,2,3,4,5,6,7,8,9,10};
int *p1 = &nums[0];
int *p2 = &nums[4];
int d = p2 - p1;
```

结果d为4，即在同一个数组中的两指针相减，则结果是两指针指向的元素索引之差。



### 4.3 比较运算符

如果两个指针指向同一个数组中的元素，我们也可以对这两个指针进行比较运算。运算结果根据指针指向不同，若指针p1指向的元素索引小于指针p2指向的元素索引，则可以说 p1 小于 p2；反之则 p2 大于 p1 。当然也可以判断两个指针是否相等，只需要确定两指针是否指向同一个位置。

注意：C语言标准允许指向数组元素的指针与指向数组最后一个元素后面的那个内存位置的指针进行比较，但不允许与指向数组第一个元素之前的那个内存位置的指针进行比较。



## 5. 其他关于指针的类型

### 5.1 指针的指针

指针的指针，就是指向指针的指针。例子：

```c
int a = 10;
int *p = &a;
int **pp = &p;
```

表示 pp 指向的地址是一个指针，该指针指向一个存储int数据的地址。

对于指针的指针，我们也可以使用取值运算符

```c
*pp
```

则 *pp 的值为 指针p 的地址，即 \*pp 等于 p。对于指针的指针，我们可以使用两个间接访问操作符：

```c
**pp --> *p --> a
```



### 5.2 指针数组与数组指针

指针数组与数组指针，虽然都是同样的四个字，但是意义却完全不同：

- 指针数组：指针数组可以说成是”存储指针的数组”，首先这个变量是一个数组，其次，”指针”修饰这个数组，意思是说这个数组的所有元素都是指针类型。
- 数组指针：数组指针可以说成是”指向数组的指针”，首先这个变量是一个指针，其次，”数组”修饰这个指针，意思是说这个指针指向一个数组的首地址。

在具体说明两者区别之前，先回顾一下优先级表（部分）：

![image-20210928213703440](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-02-25/bc38c1015dee8f9734cb77b2fe8d363b--c142--image-20210928213703440.png)

我们只需要看三个运算符：括号()、数组下标[]和取值运算符*。

括号 () 和 数组下标 [] 的优先级相同，为1，结合方向为左到右；取值运算符 * 的优先级为2，低于括号和数组下标的优先级。

- `(*p)[n]`：根据优先级和结合方向，先看括号内，则p是一个指针，这个指针指向一个一维数组，数组长度为n，这是“数组的指针”，即数组指针；

- `*p[n]`：根据优先级，先看[]，则p是一个数组，再结合*，这个数组的元素是指针类型，共n个元素，这是“指针的数组”，即指针数组。

根据上面两个分析，可以看出，优先与P结合的运算符决定了p是数组还是指针。

```c
int *p1[5]；
int (*p2)[5]；
```

首先，对于语句`int *p1[5]`，因为“[]”的优先级要比“\*”要高，所以 p1 先与“[]”结合，构成一个数组的定义，数组名为 p1，而`int *`修饰的是数组的内容，即数组的每个元素。也就是说，该数组包含 5 个指向 int 类型数据的指针，如下图所示，因此，它是一个指针数组。

![img](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-02-25/174cae1675d8192b67f780c1edba1a3e--f440--20190917164233526.jpg)

其次，对于语句`int (*p2)[5]`，根据优先级与结合方向，先看括号内，“*”号和 p2 构成一个指针的定义，指针变量名为 p2，而 int 修饰的是数组的内容，即数组的每个元素。也就是说，p2 是一个指针，它指向一个包含 5 个 int 类型数据的数组，如图 2 所示。很显然，它是一个数组指针，数组在这里并没有名字，是个匿名数组。

![img](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-02-25/e7bbd1aefc2ce879662bb9826af15320--8d55--20190917164330539.jpg)

**注意（不要被图误导！），p2 的类型是“指向数组中5个整数的指针”。指向数组第0个元素的指针和指向整个数组的指针完全不同。它们的类型不同，当 p2 为int型指针，p2++移动4个字节；当p2为int (*p)[5]型指针时，p2++移动20个字节。**



### 5.3 空指针

关于空指针的理解，有以下两种类型容易混淆：

- `int *p = NULL;`：表示指针p的类型是`int *`，但其指向的地址是一个空地址，默认为`0x00000000`，称之为空指针；
- `void* p;`：表示指针p的类型为`void *`，其可以指向任何地址，称之为void指针；

关于void指针的用法，我们可以用于函数参数、函数返回值等地方。



## 参考资料

[1] 《C和指针》

[2] 运算符优先级表：http://c.biancheng.net/cpp/html/462.html

[3] 指针数组与数组指针：https://blog.csdn.net/mick_hu/article/details/100931034

[4] 数组指针详解：https://zhuanlan.zhihu.com/p/335693933

[5] 让你不再害怕指针——C指针详解(经典,非常详细)：https://blog.csdn.net/soonfly/article/details/51131141