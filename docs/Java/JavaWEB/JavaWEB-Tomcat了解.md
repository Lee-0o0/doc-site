# Java WEB— —Tomcat详解

本文主要介绍Tomcat相关知识。



## 一、什么是Tomcat?

> The Apache Tomcat® software is an open source implementation of the Java Servlet, JavaServer Pages, Java Expression Language and Java WebSocket technologies. 

根据其官网介绍，Tomcat是一款开源软件，其实现了Java Servlet、JavaServer Pages(JSP)、Java Expression Languages以及Java WebSocket技术。

Tomcat是一款Java Web应用服务器，是用Java语言编写的，需要运行在Java虚拟机上。



## 二、Tomcat\Nginx\Apache

Nginx、Apache都是目前主流的Web服务器，也可以作为反向代理服务器；它们在处理大量并发的请求连接、连接会话管理和静态内容请求等方面相比Tomcat更具优势。

所以一般在实际应用中，先是通过Nginx（或Apache）反向代理服务器接收请求，匹配分离动态/静态请求（动静分离）;

如果是静态请求，则转发到另外的Nginx WEB服务器上，返回静态内容；如果是动态请求，则转发到后面Tomcat应用服务器，处理动态请求的业务逻辑。

简单的架构如下：

![img](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/b1e44cd27012666822e02e5e8e13ec93--fd97--20170118010505138)



## 三、Tomcat与WEB开发

Tomcat的Connector组件实现了HTTP请求的解析，Tomcat通过Connector组件接收HTTP请求并解析，然后把解析后的信息交给Servlet处理：

- 对于静态资源（html/js/jpg等）请求，Tomcat提供默认的Servlet来处理并响应；

- 对于动态请求，可以映射到自己编写的Servlet应用程序来处理。



Tomcat实现的几个Java EE规范中最重要的是Servlet，因为实现了Servlet规范，所以**Tomcat也是一个Servlet容器**，可以运行我们自己编写的Servlet应用程序来处理动态请求。

我们平时用的SpringMVC框架就是基于Servlet的，所以我们可以在这些框架的基础上进行快速开发，然后部署到Tomcat中运行。

