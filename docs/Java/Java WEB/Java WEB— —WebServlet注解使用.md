# Java WEB— —WebServlet注解使用

本文主要介绍注解WebServlet的使用。

[toc]

## 一、源码

`@WebServlet`可以替换`web.xml`中的配置。其源码如下：

```java
package javax.servlet.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface WebServlet {
    String name() default "";

    String[] value() default {};

    String[] urlPatterns() default {};

    int loadOnStartup() default -1;

    WebInitParam[] initParams() default {};

    boolean asyncSupported() default false;

    String smallIcon() default "";

    String largeIcon() default "";

    String description() default "";

    String displayName() default "";
}

```



## 二、常用属性解析

### 2.1 Target

```java
@Target({ElementType.TYPE})

public enum ElementType {
    /** Class, interface (including annotation type), or enum declaration */
    TYPE,
    ...
}
```

`@WebServlet`作用范围是类、接口或枚举类。通常用于`Servlet`实现类上。



### 2.2 常用属性

`@WebServlet`常用属性如下：

|       属性        |      类型      | 是否必须 |                   说明                    |
| :---------------: | :------------: | :------: | :---------------------------------------: |
|  asyncSupported   |    boolean     |    否    |      指定Servlet是否支持异步操作模式      |
|    displayName    |     String     |    否    |            指定Servlet显示名称            |
|    initParams     | WebInitParam[] |    否    |              配置初始化参数               |
|   loadOnStartup   |      int       |    否    | 标记容器是否在应用启动时就加载这个Servlet |
|       name        |     String     |    否    |              指定Servlet名称              |
| urlPatterns/value |    String[]    |    否    | 这两个属性作用相同，指定Servlet处理的url  |

案例如下：

```java
@WebServlet(name = "myServlet", 
	urlPatterns = "/test", 
	loadOnStartup = 1,  
	initParams = {
			@WebInitParam(name="name", value="小明"), 
			@WebInitParam(name="pwd", value="123456")
	}
)
public class MyServlet extends HttpServlet {
	......
}
```



## 三、说明

**(1).loadOnStartup属性**：标记容器是否在启动应用时就加载Servlet，默认不配置或数值为负数时表示客户端第一次请求Servlet时再加载；0或正数表示启动应用就加载，正数情况下，数值越小，加载该Servlet的优先级越高；

**(2)**. **name属性**：可以指定也可以不指定，通过getServletName()可以获取到，若不指定，则为Servlet的完整类名，如：cn.lee.MyServlet;

**(3).urlPatterns/value属性**： String[]类型，可以配置多个映射，如：urlPatterns={"/test", "/example"};

**(4).在使用注解方式时，需要注意**：

- `<web-app> </web-app>`根元素中不能配置属性metadata-complete="true"，否则无法加载Servlet。metadata-complete属性表示通知Web容器是否寻找注解，默认不写或者设置false，容器会扫描注解和Web分片，为Web应用程序构建有效的元数据；设置true，表示将由部署描述符为Web程序提供所有的配置信息;
- web.xml中不能再配置该Servlet;

**(5).urlPatterns的常用规则**：

- /*或者/：拦截所有
- *.do：拦截指定后缀
- /user/test：拦截路径
- /user/*.do、/*.do、test*.do都是**非法的**，启动时候会报错

**(6).urlPatterns的配置规则**：精确匹配、扩展名匹配、路径匹配