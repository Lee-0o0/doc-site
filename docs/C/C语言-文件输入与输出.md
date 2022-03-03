# C语言— —文件输入与输出

本文主要介绍如何在C语言中操作文件。



## 1. 文件与IO流

很多教程都说文件分为文本文件和二进制文件，那如何区分是文本文件还是二进制文件呢？现有如下的数据文件：

![image-20220226184918219](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-03-03/f3e0f3874da04141dccd46d5f9a0dc33--509f--image-20220226184918219.png)

你说它是文本文件还是二进制文件呢？

计算机存储数据，都是以二进制的形式存储。所以，在我看来，文件并没有文本文件和二进制文件的区分。

在C语言中，每个文件对应着一个数据流，这个流分为文本流和二进制流。所谓二进制流，你可以把它看成是一长串`0、1`字符串；所谓文本流，你可以把它看成是一长串字符，字符可能为`a`，`,`等。文本流就是在二进制流的基础上，加上编码，将二进制字符串解释为字符。

对于文件操作，我们可以按照如下三个步骤进行操作：

- 打开文件：所谓打开文件，就是创建一个流，使用函数`fopen()`创建流；
- 操作流：打开文件后，我们就得到了一个流，我们可以对这个流进行读数据或写数据；
- 关闭流：当操作完成后，我们需要关闭流，使用函数`fclose()`关闭流；

![image-20220303202142439](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-03-03/fcf9d8f1ae8c830903db2af9979a3a76--258a--image-20220303202142439.png)

在上图中，我们还可以看到有缓冲区，缓冲区是在内存中的，是为了快速操作数据的。以输出缓冲区为例，现在假设程序向文件输出一段话，首先程序会先输出到缓冲区中，只有输出缓冲区满了，缓冲区内的内容才会物理写入到文件中，这样会比程序直接把内容写入文件效率高。对于输入缓冲区，首先从文件中读取一块数据到缓冲区中，然后程序从缓冲区中读取数据，而不是直接从文件中读取数据。



## 2. 操作文件的函数

### 2.1 创建流

创建流的函数是`fopen()`，函数声明如下：

```c
FILE * fopen ( const char * filename, const char * mode );
```

该函数需要两个参数：

- 第一个参数`filename`表示要打开的文件路径，可以包含目录名，如果不包含路径就表示程序运行的当前目录。
- 第二个参数`mode`表示创建流的模式，**如果创建的是文本流**，有如下值：
    - `r`：是`read`的缩写，表示打开文件用于读取文件中的内容。该模式要求文件必须存在。
    - `w`：是`write`的缩写，表示打开文件用于写入内容（覆盖）。如果`filename`不存在，则创建新文件并写入内容；如果`filename`存在，则会覆盖原文件内容。
    - `a`：是`append`的缩写，表示打开文件并写入内容（追加），与`w`模式最大的不同就是`a`会在原文件内容的末尾追加写入新的内容，并不会覆盖原内容。如果文件不存在，则会创建新文件。
    - `r+`：是`read/update`的含义，表示打开文件用于读和写。该模式要求文件必须存在。
    - `w+`：是`write/update`的含义，表示打开文件用于读和写。如果`filename`不存在，则创建新文件并写入内容；如果`filename`存在，则会覆盖原文件内容。
    - `a+`：是`append/update`的含义，表示打开文件用于读和追加写。如果文件不存在，则会创建新文件。
- 第二个参数`mode`还有另外一组值，**如果创建的是二进制流**，那么分别是`rb`，`wb`，`ab`，`rb+`，`wb+`，`ab+`；

该函数返回`FILE *`类型，用于访问一个流。



### 2.2 操作流

流分为文本流和二进制流，所以此处也分开讲这两种流的操作。

#### 2.2.1 操作文本流

操作文本流，按操作对象可以分为操作单个字符或操作字符串。

如果是读写字符，那么对应的函数是：

```c
int fgetc ( FILE * stream );
int fputc ( int character, FILE * stream );
```

`fgetc()`用于从字符流`stream`中读取一个字符，如果读取成功，则返回读取的字符；否则返回`EOF`(`EOF`是`End Of File`的缩写，是一个宏定义，是一个负数常量表达式，通常为-1)。

`fputc()`用于向`stream`流中写入一个字符`character`，如果写入成功，则返回写入的字符；否则返回`EOF`。注意，`character`是一个整型变量，但是在写入文件时，会默认转换为`unsigned char`型写入。

下面的例子演示了将`你好，世界！`一个字符一个字符地写入文件中：

```c
# include <stdio.h>
# include <stdlib.h> 
# include <string.h>

int main(){
	char words[] = "你好，世界！";
	FILE *test = fopen("./test","w");
	if(test==NULL){
		perror("open test failed");
		exit(EXIT_FAILURE);
	}
	for(int i = 0; i < strlen(words); i++){
		fputc(words[i],test);
	}
	fflush(test);
	if(fclose(test) == EOF){
		perror("close test failed");
		exit(EXIT_FAILURE);
	}
	return 0;
}
```

如果是读取字符串，那么我们可以使用以下函数：

```c
char * fgets ( char * str, int buffer_size, FILE * stream );
int fputs ( const char * str, FILE * stream );
```

`fgets()`函数用于从字符流`stream`中读取字符串并复制到`str`中，`str`末尾会加上一个`\0`字符使得`str`从未字符串。如果读取到一个换行符或到文件末尾了，就不再读取了（注意，此时换行符是会读取并保存在`str`中的）；如果读取的字符数达到了`buffer_size-1`，也不再读取了。如果读取成功，返回读取到的字符串指针；如果读取失败，返回NULL。

`fputs()`函数用于向字符流`stream`写入字符串`str`，`str`包含什么就写入什么，包括换行符。写入成功，则返回一个非负数；否则返回一个负数。

下面是一个逐行读取文件内容的程序，文件内容（编码为GBK）如下：

![image-20220302210450751](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-03-03/5bb9a1541268362ef8432b59a287b87c--58fe--image-20220302210450751.png)

```c
# include <stdio.h>
# include <string.h>
# include <stdlib.h>

int main(){
	FILE *test = fopen("./test","r");
	if(test == NULL){
		perror("open test failed");
		exit(EXIT_FAILURE);
	}
	char str[100];
	while(fgets(str,100,test) != NULL){
		printf("%s",str);
	};
	fclose(test);

	return 0;
}
```

控制台输出结果：

![image-20220302210815889](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-03-03/cd88a1c90e9088d7f55de4e612a51e82--0010--image-20220302210815889.png)



#### 2.2.2 操作二进制流

二进制流的读取使用函数`fread()`，写入数据到二进制流中使用函数`fwrite()`，函数声明如下：

```c
size_t fread ( void * ptr, size_t size, size_t count, FILE * stream );
size_t fwrite ( const void * ptr, size_t size, size_t count, FILE * stream );
```

- `ptr`是一个指向用于保存数据的内存位置的指针；
- `size`是每个元素的字节数；
- `count`是要写入多少个元素；
- `stream`是要读取或写入的流；
- 函数返回值是实际读取或写入的元素数目（不是字节数），如果输入过程中遇到了文件结尾或输出过程中出现了错误，这个数字可能比请求的元素数目小；

接下来就以一个实际的例子演示读写字节流的函数使用：

```c
# include <stdio.h>
# include <stdlib.h>
# include <string.h>

int main(){
	FILE *file = fopen("./data","ab+");
	if(file ==NULL){
		perror("open ./data failed");
		exit(EXIT_FAILURE);
	}
	char data[100];
	fread(data,1,100,file);
	printf("%s",data);
	
	char output_data[] = "\n新写入的数据~~~";
	fwrite(output_data,sizeof(char),strlen(output_data),file);
	
	int res = fclose(file);
	if(res == EOF){
		perror("");
	}
	return 0;
} 
```

上面的程序首先打开程序同目录下的`data`文件，打开方式为`ab+`，表示获取的是可读可写的字节流。然后读取该流的内容进`data`字符数组，并在控制台中输出文件内容。最后，写入一些数据到文件中，并关闭文件。

控制台输出如下：

```txt
hello world!
你好，世界！
```

程序运行后，文件内容如下：

![image-20220302201827628](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-03-03/87d3fcba056e308369cf5580dda6f8f0--2c21--image-20220302201827628.png)



### 2.3 关闭流

关闭流的函数是`fclose()`，函数声明如下：

```c
int fclose ( FILE * stream );
```

该函数接受一个`FILE`指针类型参数`stream`，表示需要关闭的流。如果流成功关闭，则返回0；否则，返回`EOF`（-1）。



## 3. 刷新和定位函数

刷新函数是`fflush()`，它迫使一个输出流的缓冲区内的数据进行物理输出，不管它是不是已经写满，函数声明如下：

```c
int fflush(FILE *stream);
```

如果刷新成功，则返回0；否则返回`EOF`。

定位函数是指我们可以在流中进行定位，以改变下一个读取的字符或写入的地方，即在流中移动游标：

![image-20220303201744180](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-03-03/19463c3bac9114c934bd66b6fea86a9c--508a--image-20220303201744180.png)

定位函数主要有两个：

```c
long int ftell ( FILE * stream );
int fseek ( FILE * stream, long int offset, int from );
```

`ftell()`函数用来返回游标在流的当前位置，即距离流的起始位置的偏移量offset。

`fseek()`函数用于重定位游标，参数`from`有如下三种取值：

|   取值   |     说明     |                    与offset配合                    |
| :------: | :----------: | :------------------------------------------------: |
| SEEK_SET | 流的起始位置 | 从流的起始位置偏移offset个字节，offset必须为非负数 |
| SEEK_CUR | 流的当前位置 |   从流的当前位置偏移offset个字节，offset可正可负   |
| SEEK_END | 流的结束位置 |   从流的结束位置偏移offset个字节，offset可正可负   |

对于字节流，游标新位置可以通过`from`和`offset`进行计算得出；

对于字符流，`from`必须为`SEEK_SET`，并且`offset`的值为之前调用`ftell()`函数的返回值。如果`from`的值为`SEEK_CUR`或`SEEK_END`，则`offset`必须为0。



## 参考资料

[1] 汉字-编码转换在线网站：https://www.qqxiuzi.cn/bianma/guojima.php

[2] stdio库函数介绍：https://www.cplusplus.com/reference/cstdio/

[3] 《C和指针》-第15张 输入/输出函数