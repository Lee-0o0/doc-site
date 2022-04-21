# CSS— —入门
本文主要介绍CSS入门知识。


## 1. 什么是CSS
CSS，全称Cascading Style Sheets，译为层叠样式表。有什么用呢？让网页更好看。


## 2. CSS的语法
CSS的基本单位由选择器和规则（属性）表组成，语法格式如下：
```txt
选择器 {
    key: value; /* 一条规则 */
    key: value; /* 一条规则 */
    ...
}
```
一条规则就是一个键值对（属性: 值）。


## 3. CSS引入
我们有四种方法在我们的网页中引入CSS。

### 3.1 行内样式
行内样式存在于HTML元素的`style`属性之中。其特点是每个CSS只影响一个元素。例如：
```html
<h1 style="color: blue;background-color: yellow;border: 1px solid black;">Hello World!</h1>
<p style="color:red;">This is my first CSS example</p>
```

### 3.2 内部样式表
内部样式表将CSS放在HTML文件`<head>`标签里的`<style>`标签之中。例如：
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
    <style>
      h1 {
        color: blue;
        background-color: yellow;
        border: 1px solid black;
      }

      p {
        color: red;
      }
    </style>
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>This is my first CSS example</p>
  </body>
</html>
```


### 3.3 外部样式表
外部样式表是指将CSS编写在扩展名为.css 的单独文件中，并在HTML中通过`<link>`元素引用它。例如：
首先存在一个单独的CSS文件
```css
h1 {
  color: blue;
  background-color: yellow;
  border: 1px solid black;
}

p {
  color: red;
}
```
然后在HTML文件中引入独立的CSS文件：
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
    <link rel="stylesheet" href="styles.css" type="text/css">
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>This is my first CSS example</p>
  </body>
</html>
```

`<link>`元素定义文档与外部资源的关系，最常见的用途就是链接样式表。有以下属性：
- href：定义外部资源（被链接文档）的位置。
- rel：定义当前文档与被链接文档之间的关系。rel 是 relationship的英文缩写。可取stylesheet、icon等值。
- type：规定被链接文档的 MIME 类型。


## 参考资料
[1] `<link>`元素: https://www.runoob.com/tags/tag-link.html