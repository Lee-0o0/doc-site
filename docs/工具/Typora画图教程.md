# Typora画图教程

本文主要介绍如何在Typora中画图。本文主要翻译自Typora官网中关于画图的[文章](http://support.typora.io/Draw-Diagrams-With-Markdown/)介绍。



## 一、概述

> Typora supports some Markdown extensions for diagrams, once they are enabled from preference panel.
>
> When exporting as HTML, PDF, epub, docx, those rendered diagrams will also be included, but diagrams features are not supported when exporting markdown into other file formats in current version. Besides, you should also notice that diagrams is not supported by standard Markdown, CommonMark or GFM. Therefore, we still recommend you to insert an image of these diagrams instead of write them in Markdown directly.

译文：

Typora支持一些Markdown图表插件，只需要在偏好设置中开启即可（设置如下）。

![image-20201120204609946](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-03-03/cc506f02ccd69659d94504fc90ef2eef--f743--image-20201120204609946.png)

当把导出为HTML、PDF、epub、docx时，被渲染好的图表也会被导出，但是在当前版本（原文发表于2016.8.15，我也不知道那时候是什么版本）中，图表特性在导出文件中不被支持。另外，你应该注意图表在标准Markdown、CommonMark或GFM中不被支持。因此，我们仍然建议你将这些图表作为图像插入到文章中，而不是直接用Markdown绘制。



## 二、开始画图

### 2.1 序列图

> This feature uses [js-sequence](https://bramp.github.io/js-sequence-diagrams/), which turns the following code block into a rendered diagram:

这个特性使用了`js-sequence`，它能将以下代码块转换为渲染好的图表：

```txt
```sequence
title : 这是标题
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
Bob ->> Henry: Hello Henry, How are you?
​```
```

```mermaid
sequenceDiagram
title : 这是标题
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
Bob ->> Henry: Hello Henry, How are you?
```

相信你能看出一些基本元素的语法，了解更多语法规则，请查看以下链接：

[1]https://bramp.github.io/js-sequence-diagrams/

[2]https://github.com/bramp/js-sequence-diagrams/blob/master/src/grammar.jison



### 2.2 流程图

> This feature uses [flowchart.js](http://flowchart.js.org/), which turns the following code block into a rendered diagram:

这个特性使用了`flowchart.js`，它能将下列代码块转换为图表：

````txt
```flow
%% 注释
%% 将起始点start命名为st(随便取),然后起始点内容为Start(冒号后的文本)
%% :>超链接地址,当点击该元素时，会跳转到相应网站
st=>start: Start:>http://www.lee-0o0.com
op1=>operation: Your Operation1
op2=>operation: Your Operation2:>http://www.lee-0o0.com
cond=>condition: Yes or No?
io=>inputoutput: catch something...
e=>end

st->op1->op2->io->cond
cond(yes)->e
cond(no)->op1
```
````

![image-20220303210203198](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-03-03/146862fe4527e7b7261093ceec86bf5f--6ecf--image-20220303210203198.png)


关于更多的案例，请查看以下链接：

[1]http://flowchart.js.org/



### 2.3 Mermaid

> Typora also has integration with [mermaid](https://mermaid-js.github.io/mermaid/#/), which supports sequence diagrams, flowcharts, Gantt charts, class and state diagrams, and pie charts.

Typora同样支持`Mermaid`，它将序列图、流程图、甘特图、类图、状态图和饼图整合在一起。

了解更多语法规则，查看以下链接：

[1]https://mermaid-js.github.io/mermaid/#/



#### 2.3.1 序列图

```txt
```mermaid
sequenceDiagram
    Alice->>Bob: Hello Bob, how are you?
    alt is sick
    Bob->>Alice: Not so good :(
    else is well
    Bob->>Alice: Feeling fresh like a daisy
    end
    opt Extra response
    Bob->>Alice: Thanks for asking
    end
​```
```

```mermaid
sequenceDiagram
    Alice->>Bob: Hello Bob, how are you?
    alt is sick
    Bob->>Alice: Not so good :(
    else is well
    Bob->>Alice: Feeling fresh like a daisy
    end
    opt Extra response
    Bob->>Alice: Thanks for asking
    end
```



#### 2.3.2 流程图

````txt
```mermaid
graph LR
A[Hard edge] -->B(Round edge)
    B --> C{Decision}
    C -->|One| D[Result one]
    C -->|Two| E[Result two]
```
````

```mermaid
graph LR
	A[Hard edge] -->B(Round edge)
    B --> C{Decision}
    C -->|One| D[Result one]
    C -->|Two| E[Result two]
```



#### 2.3.3 甘特图

````txt
```mermaid
%% Example with selection of syntaxes
        gantt
        dateFormat  YYYY-MM-DD
        title Adding GANTT diagram functionality to mermaid

        section A section
        Completed task            :done,    des1, 2014-01-06,2014-01-08
        Active task               :active,  des2, 2014-01-09, 3d
        Future task               :         des3, after des2, 5d
        Future task2               :         des4, after des3, 5d

        section Critical tasks
        Completed task in the critical line :crit, done, 2014-01-06,24h
        Implement parser and jison          :crit, done, after des1, 2d
        Create tests for parser             :crit, active, 3d
        Future task in critical line        :crit, 5d
        Create tests for renderer           :2d
        Add to mermaid                      :1d

        section Documentation
        Describe gantt syntax               :active, a1, after des1, 3d
        Add gantt diagram to demo page      :after a1  , 20h
        Add another diagram to demo page    :doc1, after a1  , 48h

        section Last section
        Describe gantt syntax               :after doc1, 3d
        Add gantt diagram to demo page      : 20h
        Add another diagram to demo page    : 48h
```
````

```mermaid
        gantt
        dateFormat  YYYY-MM-DD
        title Adding GANTT diagram functionality to mermaid

        section A section
        Completed task            :done,    des1, 2014-01-06,2014-01-08
        Active task               :active,  des2, 2014-01-09, 3d
        Future task               :         des3, after des2, 5d
        Future task2               :         des4, after des3, 5d

        section Critical tasks
        Completed task in the critical line :crit, done, 2014-01-06,24h
        Implement parser and jison          :crit, done, after des1, 2d
        Create tests for parser             :crit, active, 3d
        Future task in critical line        :crit, 5d
        Create tests for renderer           :2d
        Add to mermaid                      :1d

        section Documentation
        Describe gantt syntax               :active, a1, after des1, 3d
        Add gantt diagram to demo page      :after a1  , 20h
        Add another diagram to demo page    :doc1, after a1  , 48h

        section Last section
        Describe gantt syntax               :after doc1, 3d
        Add gantt diagram to demo page      : 20h
        Add another diagram to demo page    : 48h
```



#### 2.3.4 类图

````txt
```mermaid
classDiagram
      Animal <|-- Duck
      Animal <|-- Fish
      Animal <|-- Zebra
      Animal : +int age
      Animal : +String gender
      Animal: +isMammal()
      Animal: +mate()
      class Duck{
          +String beakColor
          +swim()
          +quack()
      }
      class Fish{
          -int sizeInFeet
          -canEat()
      }
      class Zebra{
          +bool is_wild
          +run()
      }
```
````

```mermaid
classDiagram
      Animal <|-- Duck
      Animal <|-- Fish
      Animal <|-- Zebra
      
      class Animal{
      	  +age:int
      	  +gender:String
      	  +isMamal()
      	  +mate()
      }
      class Duck{
          +String beakColor
          +swim()
          +quack()
      }
      class Fish{
          -int sizeInFeet
          -canEat()
      }
      class Zebra{
          +bool is_wild
          +run()
      }
```



#### 2.3.5 状态图

````txt
```mermaid
stateDiagram
    [*] --> Still
    Still --> [*]

    Still --> Moving
    Moving --> Still
    Moving --> Crash
    Crash --> [*]
```
````

```mermaid
stateDiagram
	[*] --> Still
    Still --> [*]

    Still --> Moving
    Moving --> Still
    Moving --> Crash
    Crash --> [*]
```



#### 2.3.6 饼图

````txt
```mermaid
pie
	title 饼图标题
	"dogs":156
	"cats":100
	"birds":60
```
````

```mermaid
pie
	title 饼图标题
	"dogs":156
	"cats":100
	"birds":60
```





## 三、参考资料

[1]http://support.typora.io/Draw-Diagrams-With-Markdown/

