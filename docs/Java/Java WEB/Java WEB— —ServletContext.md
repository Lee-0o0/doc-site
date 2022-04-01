# Java WEB— —ServletContext

本文主要介绍ServletContext的相关知识。

[toc]

## 一、什么是ServletContext

`ServletContext`是一个接口：

```java
public interface ServletContext {
    ...
}
```

但`ServletContext`的对象由Web容器在部署项目时创建。

`ServletContext`官方叫servlet上下文。服务器会为每一个工程创建一个对象，这个对象就是`ServletContext`对象。这个对象全局唯一，而且工程内部的所有servlet都共享这个对象，所以叫全局应用程序共享对象。

我们可以使用接口`ServletConfig`中的方法`getServletContext()`获取`ServletContext`对象：

```java
public interface ServletConfig {
    ServletContext getServletContext();
}
```

```java
ServletContext servletContext = this.getServletConfig().getServletContext();
```



## 二、作用

### 2.1 存取数据

ServletContext是一个域对象。所谓域对象，就是可以像map一样存储数据。

| 方法                                    | 说明                                            |
| --------------------------------------- | ----------------------------------------------- |
| setAttribute(String name,Object value); | 往域对象里面添加数据，添加时以key-value形式添加 |
| getAttribute(name);                     | 根据指定的key读取域对象里面的数据               |
| removeAttribute(name);                  | 根据指定的key从域对象里面删除数据               |



### 2.2 获取全局配置参数

我们可以在web.xml文件中配置全局参数：

```xml
<context-param>
    <param-name>username</param-name>
    <param-value>xiaoming</param-value>
</context-param>
<context-param>
    <param-name>password</param-name>
    <param-value>1234</param-value>
</context-param>
```

然后使用`ServletContext`获取参数：

```java
ServletContext servletContext = this.getServletConfig().getServletContext();
String username = servletContext.getInitParameter("username");
String password = servletContext.getInitParameter("password");
```



### 2.3 获取当前工程名

我们可以通过下列方法获取当前工程名：

```java
ServletContext servletContext = this.getServletConfig().getServletContext();
String contextPath = servletContext.getContextPath();
```



### 2.4 获取工程在服务器上的路径

```java
ServletContext servletContext = this.getServletConfig().getServletContext();
String realPath = servletContext.getRealPath("/");
```

关于参数的说明：

`ServletContext.getRealPath() `是从当前项目在tomcat 中的存放文件夹开始计算起的。

比如，有个项目叫 UploadServlet，它部署在tomcat 下面以后的绝对路径如下：

```txt
C:\Program Files\apache-tomcat-8.0.3\webapps\UploadServlet
```

那么，`ServletContext.getRealPath("/")` 返回 :

```txt
C:\Program Files\apache-tomcat-8.0.3\webapps\UploadServlet
```

`ServletContext.getRealPath("/attachment") `返回：

```txt
C:\Program Files\apache-tomcat-8.0.3\webapps\UploadServlet\attachment
```

`ServletContext.getRealPath("attachment") `会导致NullPointerException。

根据Tomcat版本不同，结果可能会不同。





