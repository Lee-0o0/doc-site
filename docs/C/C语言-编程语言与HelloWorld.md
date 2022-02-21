# C语言— —编程语言与Hello World!

本文主要介绍编程语言的基本知识以及带大家编写C语言版的Hello World!入门程序。


## 1. 编程语言

当我们开始学习编程时，首先学习的就是编程语言，那么自然而然，什么是编程语言？

![image-20210808115346154](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/49ceffcfdf35d5fb6d16a16b93f56ff3--101e--image-20210808115346154.png)

简单理解，编程语言就是我们和计算机交流的工具，就像英语、法语其它语言一样。

编程语言按照阶段可以分为以下几类：

- 机器语言：只有用机器语言编写的程序才能被计算机直接理解和执行（举个例子，美国人只能看懂英语写的书），机器语言就是一串的0、1，例如：00101010 10101 01111000，计算机能看懂这串01代表什么意思。那么问题来了，我们能看懂吗？可以看懂，但不直观，学习成本高。
- 汇编语言：由于机器语言难读、难编、难记和易出错的缺点，所以出现了汇编语言，汇编语言的特点是用符号代替了机器指令代码，就是用符号来代替了01串。例如，以上面的01串为例，用汇编语言写，可能会是这样的：**42**(00101010) **ADD**(10101) **120**(01111000)，所以上面的01串说的是42+120。但是汇编语言也存在着通用性差的原因，即不同型号的计算机，需要使用不同的汇编语言，所以出现了高级语言。
- 高级语言：高级语言是面向用户的语言，即高级语言与人类语言类似，自然直观、通熟易懂、简单易学。目前常见的高级语言有C、C++、Java、C#、Python等。

我们能轻松掌握的是高级语言，但是计算机能理解执行的是机器语言，所以在高级语言和机器语言之间存在着一个转换过程，如下图：

![image-20210808182134475](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/329a7d251fb1468716850797c825fb6d--e949--image-20210808182134475.png)

就像我们只会说普通话，如果需要与美国人交流，那么就需要翻译者将普通话翻译为英语，这样我们才能和美国人交流。同样的，我们用高级语言编写的程序，需要经过翻译者，翻译为机器语言对应的程序，这样计算机才能理解执行。

这种翻译存在两种方式：编译和解释。

编译方式：用户用高级语言编写的源程序，经过编译程序“翻译”后，输出目标程序，称为可执行文件，计算机可以运行目标程序。C语言采用编译方式。

解释方式：解释程序一条一条地解释用高级语言编写的源程序，并不会产生目标程序。Python采用解释方式。

以上简单介绍了编程语言的分类以及编译和解释，相信大家对于编程也不会感到是一件非常艰难的事了（注意，此处的编译是一个笼统的概念）。



## 2. C语言历史

为什么要介绍C语言历史呢？因为不同版本的C语言支持的语言特性不一样，我们通过关注C语言的版本来了解不同版本的差异，使得我们的程序更健壮。

C语言的发展与Unix操作系统的发展紧密相关。Unix操作系统是由Dennis Ritchie和Ken Thompson用汇编语言写的。

![img](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/70c4200a77e3da71847ad37703961461--10ba--220px-Ken_Thompson_and_Dennis_Ritchie--1973.jpg)

<p align=center><font color=gray>Ken Thompson(左) 和 Dennis Ritchie (右)（图源：维基百科）</font></p>

最初，Thompson 希望发明一门编程语言，可以在Unix操作系统上编写程序。在BCPL编程语言的基础上，Thompson提出了一个简化版，称为B语言。但是，B语言太慢了，导致了B语言的失败。

1972年，Dennis Ritchie改进了B语言，提出了C语言。这就是C语言的诞生。

1978年， Brian Kernighan 和 Dennis Ritchie提出了C编程语言第一版规范，称为 `K&R C`。

![Brian Kernighan in 2012 at Bell Labs 1.jpg](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/84da35cb1d33ba087137046d1c3e0dd7--b436--220px-Brian_Kernighan_in_2012_at_Bell_Labs_1.jpg)

<p align=center><font color=gray>Brian Kernighan（图源：维基百科）</font></p>

1989年，美国国家标准协会（American National Standards Institute, ANSI)批准ANSI X3.159-1989为C标准，该版本通常被称为ANSI C、标准C，有时也称为C89。

1990年，国际标准化组织( International Organization for Standardization, ISO)采用了ANSI C标准(格式有变化)，称为C90。因此，术语C89和C90指的是同一种编程语言。

1999年，进一步修订的C标准出版，通常被称为C99。

2011年12月8日，C语言的另一个修订版本出版，称为C11。

2018年6月，C17发布，是C编程语言的当前标准。

C2x是下一个(继C17之后)主要的C语言标准修订版的非正式名称。



## 3. Hello World!

首先介绍以下编写C语言程序的步骤：

![image-20210808202257776](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/bd4c2750429ee5b4e4f19975f27fa7f9--6bec--image-20210808202257776.png)

如上图，我们需要文本编辑器和编译器，只需要下载**dev-cpp**，那么我们就有了文本编辑器和编译器。

编译器支持的C语言版本也可能不同，有的时候不同的语法在不同的编译器中表现不一样，所以一定要注意你的编译器支持的C语言版本。关于C语言编译器列表，参考资料[5]。



### 3.1 下载dev-cpp

下载地址：https://sourceforge.net/projects/orwelldevcpp/

下载地址2-腾讯软件中心：https://pc.qq.com/detail/16/detail_163136.html

建议使用下载地址2进行下载。

下载完成安装后，打开界面如下：

![image-20210808203012004](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/df38540318fbaa77474e09df34c80302--9aaf--image-20210808203012004.png)



### 3.2 编写源程序

依次点击**文件-->新建-->源代码**（或使用快捷键`Ctrl+N`，或点击文件下的图标![image-20210808203229216](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/339ba9935355353edd20bd3a9b4de09c--8d9c--image-20210808203229216.png)选择源代码），创建一个新的源文件：

![image-20210808203324748](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/d5331438836d4b6ac17369aebc427f00--cad8--image-20210808203324748.png)

然后我们就可以在源文件中编写如下C语言程序了：

```c
# include <stdio.h>
int main()
{
	printf("Hello World!");
	return 0;	
}
```

![image-20210808203507056](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/c4595d92015e3e4ac4ca7a2d9e72e865--37b8--image-20210808203507056.png)

编写完成后，快捷键`Ctrl+S`保存源文件，选择源文件的保存位置，命名源文件，选择后缀名（此处选择后缀名为.c）：

![image-20210808203755272](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/f3d39b83aa2849466d086edb23f67973--8bf6--image-20210808203755272.png)

当保存后，就可以在我们选择的位置处发现我们的源文件（源程序）：

![image-20210808203828227](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/02a5c17486da5c9fd5fd5218e285c101--5998--image-20210808203828227.png)

### 3.3 编译

当我们编写源程序后，需要将其编译为可执行文件，在IDE中依次点击**运行-->编译（或快捷键F9）**，结果如下：

![image-20210808204125171](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/d02b99d7e28dde4438b5202564d61cd1--cc1a--image-20210808204125171.png)

编译的结果是在源文件同目录下生成了一个同名的可执行文件（后缀名为.exe，exe为execute的缩写）：

![image-20210808204306835](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/6fe42c92b32a94ea041afefa4a2b5f8e--2b84--image-20210808204306835.png)



### 3.4 执行

我们可以直接双击`helloworld.exe`，执行该目标程序，你会发现电脑屏幕有一个东西一闪而过，其实这就是该目标程序执行完成然后退出的过程。

我们在dev-cpp中依次点击**运行-->运行(或快捷键F10)**，运行结果如下：

![image-20210808204704589](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-10-05/137c901564b70bcd43a829dfe0971b09--d150--image-20210808204704589.png)

可以看到一个黑框框中出现了`Hello World!`，这个黑框框我们称之为控制台。这就是第一个C语言程序，输出`Hello World!`。



## 参考资料

[1] 百度百科-计算机语言：https://baike.baidu.com/item/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BC%96%E7%A8%8B%E8%AF%AD%E8%A8%80/5581937

[2] 维基百科-编译器：https://zh.wikipedia.org/wiki/%E7%B7%A8%E8%AD%AF%E5%99%A8

[3] 维基百科-解释器：https://zh.wikipedia.org/wiki/%E7%9B%B4%E8%AD%AF%E5%99%A8

[4] 维基百科-C语言：https://en.wikipedia.org/wiki/C_(programming_language)#History

[5] C语言编译器：https://en.wikipedia.org/wiki/List_of_compilers#C_compilers