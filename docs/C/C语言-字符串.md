# C语言— —字符串

本文主要介绍C语言中的字符串。



## 1. 什么是字符串

在C语言中，字符串是以空字符`\0`结尾的一维字符数组。

空字符又称结束符，是一个数值为 0 的控制字符，`\0` 是转义字符，意思是告诉编译器，这不是字符 `0`，而是空字符。



## 2. 字符串的声明

我们可以通过声明并初始化字符数组来声明字符串：

```c
char str[] = {'h','e','l','l','o','\0'};
```

注意，最后一个字符为空字符。

当然，我们还可以用更简化的语法声明字符串：

```c
char str[] = "hello";
```

使用这种方式声明字符串，编译器会自动在最后加上一个空字符。



## 3. 字符串相关函数

我们可以引入头文件`<string.h>`，来使用一些关于字符串的函数。

### 3.1 strlen()函数

函数声明如下：

```c
size_t strlen ( const char * str );    // size_t是unsigned int的别名
```

`strlen(str)`函数用于计算字符串`str`的长度：

```c
# include <stdio.h>
# include <string.h>

int main(){
	char str[] = "hello";
	
	int length = strlen(str);
	printf("%d\n",length);
	
	return 0;
} 
```

输出：`5`

注意：字符串的长度不包括结尾的空字符。



### 3.2 strcat()函数

函数声明如下：

```c
char * strcat ( char * destination, const char * source );
```

`strcat(destination, source)`函数用于连接两个字符串，将`source`字符串附加在`destination`后面，并返回`destination`：

```c
# include <stdio.h>
# include <string.h>

int main(){
	char hello[30] = "hello";
	char world[] =" world！"; 
	
	strcat(hello, world);
	printf("%s\n", hello);
	
	return 0;
} 
```

输出：`hello world！`

注意：`destination`需要有足够的空间容纳`destination`+`source`；



### 3.3 strcpy()函数

函数声明如下：

```c
char * strcpy ( char * destination, const char * source );
```

`strcpy(destination, source)`函数用于将`source`字符串复制进`destination`中。例子：

```c
# include <stdio.h>
# include <string.h>

int main(){
	char hello[30] = "hello";
	char world[] =" world！"; 
	
	strcpy(hello, world);
	printf("%s\n", hello);
	
	return 0;
} 
```

输出：` world！`

同理，`destination`需要有足够的空间容纳`source`。



### 3.4 strupr()和strlwr()函数

- `strupr(str)`函数用于将`str`中的字母转换为大写字母；
- `strlwr(str)`函数用于将`str`中的字母转换为小写字母；

例子：

```c
# include <stdio.h>
# include <string.h>

int main(){
	char hello[] = "Hello";
	char world[] = "World！"; 
	
	strupr(hello);
	strlwr(world); 
	printf("%s\n", hello);
	printf("%s\n", world);
	return 0;
} 
```

输出：

```txt
HELLO
world！
```

`strupr()`和`strlwr()`不是标准库函数，只能在Windows下（VC、MinGW等）使用，Linux GCC中需要自己定义。



关于更多关于字符串的函数，请参考资料[1]。



## 参考资料

[1] https://www.cplusplus.com/reference/cstring/

[2] http://c.biancheng.net/cpp/html/2716.html