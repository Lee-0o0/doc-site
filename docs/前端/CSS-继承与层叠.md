# CSS— —继承与层叠
本文介绍CSS中的继承与层叠。



## 1. 继承
继承是指给父元素设置某些CSS属性，那么这些属性也会应用在子元素上。
例如：
```html
<!-- css -->
<style type="text/css">
    .father { 
    	 width: 300px;            /* 设置宽度 */
    	 font-size: 20px;         /* 设置字体大小 */
    	 text-align: right;       /* 字体右对齐 */
    	 background-color: green; /* 背景颜色为绿色 */
    	 color:red;               /* 字体颜色为红色 */
     }
</style>
 
<!-- html -->
<body>
    <div class="father">
        father标签
        <p>father子标签 p</p>
    </div>
</body>
```
显示效果：

![](img\CSS-继承与层叠\Snipaste_2022-04-21_14-05-55.png)

注意：
- 继承发生的前提是HTML标签具有父子关系（嵌套关系、包含关系）；
- 并不是所有的属性都可以继承，只有以color/font-/text-/line-开头的属性才可以继承；
- 在CSS的继承中不仅仅是儿子可以继承, 只要是后代都可以继承；
- 继承性中的特殊性：
   1. `<a>`标签的文字颜色和下划线是不能继承的
   2. `<h>`标签的文字大小是不能继承的



## 2. 层叠
我所理解的层叠，是多个选择器选择了同一个HTML元素，那么HTML元素的表现效果会综合选择器的属性。例如：
```html
<!-- css -->
<style>
    span{
        font-style: italic;     /* 斜体 */
        color: blue;
    }
    span{
        color: green;
    }
</style>

<!-- html  -->
<div>
    <span>span标签内容</span>
</div>
```
结果如下：

![](img\CSS-继承与层叠\Snipaste_2022-04-21_14-16-06.png)

可以看到，`<span>`标签的字体格式为斜体，颜色为绿色。明显是两个CSS样式表综合的结果。



## 3. 优先级
在层叠的例子中，我们看到两个CSS样式表都设置了字体颜色，分别为蓝色和绿色，但最终字体颜色呈现为绿色。这就涉及优先级。所谓优先级，就是选择了同一个HTML元素的多个选择器中存在相同的属性，取优先级高的选择器中的属性。我们将在下一文中讲解选择器和优先级知识。