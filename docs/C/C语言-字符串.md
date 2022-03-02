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
char str[] = "hello";          // 方式一

char str[] = "你好"",世界";     // 方式二

char str[] = "hello world"\   // 方式三
    		 "你好，世界"\
    		 "\n新的一天开始了~";
```

使用上面的方式声明字符串，编译器会自动在最后加上一个空字符。对于方式一，很自然，没什么可说的；对于方式二，我们可以将两个字符串常量放在一起，编译器会自动将两个字符串常量拼接在一起，`str`为`你好,世界`；对于方式三，如果字符串很长，我们可以先按照方式二将其拆分为多个字符串常量，然后放在多行，并在每行的末尾加上一个反斜杠`\`，表示字符串拼接。



## 3. 字符串相关函数

我们可以引入头文件`<string.h>`，来使用一些关于字符串的函数。

### 3.1 strlen()函数

函数声明如下：

```c
size_t strlen ( const char * str );    // size_t是unsigned int的别名
```

`strlen(str)`函数用于计算字符串`str`的长度（**以字节为单位**）：

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

我们强调`strlen()`输出的字符串长度是以字节为单位，看如下例子：

```c
# include <stdio.h>
# include <string.h>

int main(){
	char str[] = "你好";
	printf("strlen(str) = %d\n",strlen(str)); 
	return 0;
}
```

输出：`strlen(str) = 4`

明明`str`字符串只包含两个字符`你`和`好`，为什么长度会为4呢？

要知道原因，你需要了解[编码](编码.md)的相关知识。

在我们的Dev-cpp中，默认的编码方式是GBK，在GBK编码中，每个汉字占据两个字节。如果我们使用[Visual Studio Code](https://code.visualstudio.com/)打开源代码，界面如下：

![image-20220302200320914](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-03-02/04815bdaa3903f735d430a19a0dcdf9c--a986--image-20220302200320914.png)

可以看到在Visual Studio Code中，是以UTF-8编码格式打开的，并且`你好`变成了乱码。如果我们在Visual Studio Code中修改`str`字符串为`你好`，再以UTF-8编码保存源文件，然后在Dev-Cpp中查看，界面如下：

![image-20220302200543981](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-03-02/45120028a09125b9683f3099c252255c--7589--image-20220302200543981.png)

编译运行后，输出结果为：`strlen(str) = 6`。这是因为此时的`你好`为UTF-8编码，每个汉字占据3个字节。

`strlen()`与`sizeof()`的区别：`strlen()`函数不包括末尾的空字符，`sizeof()`会包括末尾的空字符。



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

[3] https://blog.csdn.net/qq_43338695/article/details/97621375