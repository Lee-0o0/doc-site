# C语言— —动态内存分配

本文主要介绍C语言中的动态内存分配。



## 1. 什么是动态内存分配

所谓动态内存分配，是相对于静态内存分配的。所谓动态和静态就是指内存的分配方式，动态内存是指在堆上分配的内存，而静态内存是指在栈上分配的内存。

前面所写的程序大多数都是在栈上分配的，比如局部变量、形参、函数调用等。栈上分配的内存是由系统分配和释放的，空间有限，在复合语句或函数运行结束后就会被系统自动释放。而堆上分配的内存是由程序员通过编程自己手动分配和释放的，空间很大，存储自由^[3]^。



## 2. 动态内存分配与释放

### 2.1 动态内存分配

动态内存分配主要有以下三个方法，首先需要在程序中包含`<stdlib.h>`头文件：

```c
void* malloc (size_t size);
void* calloc (size_t element_num, size_t element_size);
void* realloc (void* ptr, size_t new_size);
```

- `malloc()`用于在堆上分配一块大小为`size`的内存块，如果分配成功，则返回指向该内存块地址的指针；如果分配失败，则返回NULL指针。注意，`malloc()`所分配的是一块连续的内存。

- `calloc()`分配一块大小为`element_num*element_size`大小的连续内存块，并对其进行初始化为0。分配成功，则返回指向该内存块的地址；分配失败，则返回NULL。

- `realloc()`用于修改一个原先已经分配的内存块的大小，`ptr`指向原内存块，`new_size`为新内存块的大小。如果想申请一块更大的内存块，那么原有的内存块内容会复制到新内存块中，新增加的内存在后面；如果申请一块更小的内存块，那么旧内存块尾部多余的部分就会被截取掉。如果分配成功，则函数返回值是新内存块的地址，该地址可能与`ptr`指向的地址相同，也可能是一个新地址；如果分配失败，则返回NULL指针，但`ptr`所指向的内存块并不会被释放。在使用`realloc()`重新分配内存块时，使用下面的方式：

    ```c
    p = realloc(p,size);
    ```

    如果`ptr`是一个NULL指针，那么`realloc()`与`malloc()`作用相同。

- 上述三个动态内存分配的函数返回值都是`void *`，因此，在使用时需要进行强制类型转换为我们需要的类型，如：

    ```c
    int *p = (int*)malloc(100);
    ```

    

### 2.2 动态内存释放

我们可以使用函数`free()`用来释放由`malloc()`、`calloc()`和`realloc()`分配的动态内存：

```c
void free (void* ptr);
```

如果`ptr`为NULL指针，那么`free()`不做任何事；如果`ptr`所指向的并不是动态内存块，`free()`会造成未定义的结果。

注意，当调用`free()`后，`ptr`的值并未改变，只不过它所指向的动态内存块已失效（变为空闲可用）。



### 2.3 案例

**案例一：输入一个班级学生的数学成绩，并计算平均数**

```c
# include <stdio.h>
# include <stdlib.h>

int main(){
	int students;
	printf("请输入班级学生人数：");
	scanf("%d",&students);
	// 申请动态内存并依次赋值 
	int *scores = (int*)calloc(students, sizeof(int));
	printf("请依次输入学生数学成绩(空格分隔)：");
	for(int i = 0; i < students; i++){
		scanf("%d",scores+i);
	}
	
	// 计算平均数，保留整数
	int totalScore = 0;
	for(int i = 0; i < students; i++) {
		totalScore += *(scores+i);
	}
	printf("班级平均数学成绩为：%d",totalScore/students);
	// 释放动态内存 
	free(scores);
	return 0;
}

```

结果：

![image-20220304211925405](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-03-06/3c0cdd16ba7bda30f842230c43341c03--6d67--image-20220304211925405.png)

你可能会想，明明用数组就能完成的事，为什么这里需要用动态内存分配呢？这是因为有的C语言版本和编译器要求声明数组时，数组长度为常量，即下面的代码是不行的：

```c
int students;
printf("请输入班级学生人数：");
scanf("%d",&students);
int scores[students];
```



**案例二：崩溃你的电脑**

```c
# include <stdio.h>
# include <stdlib.h>

int main(){
	while(true){
		int *p = (int*)malloc(1024);
	}
	return 0;
}
```

上面一段代码无限分配动态内存，但并不释放，导致计算机内存不够，电脑崩溃。

**所以，分配了动态内存，使用完后，一定要记得释放。**



## 参考资料

[1] 《C和指针》- 第11章：动态内存分配

[2] 内存堆和栈：https://www.jianshu.com/p/52b5a1879aa1

[3] 动态内存分配：http://c.biancheng.net/view/223.html
