# Java WEB— —请求与响应

本文主要介绍请求与响应的相关知识，即`HttpServletRequest`和`HttpServletResponse`。



## 一、HttpServletRequest

HttpServletRequest对象代表客户端的请求，当客户端通过HTTP协议访问服务器时，HTTP请求中的所有信息都封装在这个对象中，开发人员通过这个对象的方法，可以获得客户这些信息。

### 1.1 获取客户机信息

|           方法声明           | 功能描述                                                     |
| :--------------------------: | :----------------------------------------------------------- |
|      String getMethod()      | 该方法用于获取HTTP请求消息中的请求方式（如GET、POST等）      |
|    String getRequestURI()    | 该方法用于获取请求行中资源名称部分，即位于URL的主机和端口之后、参数部分之前的部分 |
|   String getQueryString()    | 该方法用于获取请求行中的参数部分，也就是资源路径后面问号（?）以后的所有内容 |
|     String getProtocol()     | 该方法用于获取请求行中的协议名和版本，例如，HTTP/1.0或HTTP/1.1 |
|   String getContextPath()    | 该方法用于获取请求URL中属于WEB应用程序的路径，这个路径以“/”开头，表示相对于整个WEB站点的根目录，路径结尾不含“/”。如果请求URL属于WEB站点的根目录，那么返回结果为空字符串（""） |
|   String getServletPath()    | 该方法用于获取Servlet所映射的路径(url-pattern)               |
|    String getRemoteAddr()    | 该方法用于获取请求客户端的IP地址，其格式类似于“192.168.0.3”  |
|    String getRemoteHost()    | 该方法用于获取请求客户端的完整主机名，其格式类似于“www.sw.com”。需要注意的是，如果无法解析出客户机的完整主机名，该方法将会返回客户端的IP地址 |
|    String getRemotePort()    | 该方法用于获取请求客户端网络连接的端口号                     |
|    String getLocalAddr()     | 该方法用于获取Web服务器上接收当前请求网络连接的IP地址        |
|    String getLocalName()     | 该方法用于获取Web服务器上接收当前网络连接IP所对应的主机名    |
|      int getLocalPort()      | 该方法用于获取Web服务器上接收当前网络连接的端口号            |
|    String getServerName()    | 该方法用于获取当前请求所指向的主机名，即HTTP请求消息中Host头字段所对应的主机名部分 |
|     int getServerPort()      | 该方法用于获取当前请求所连接的服务器端口号，即如果HTTP请求消息中Host头字段所对应的端口号部分 |
|      String getScheme()      | 该方法用于获取请求的协议名，例如http、https或ftp             |
| StringBuffer getRequestURL() | 该方法用于获取客户端发出请求时的完整URL，包括协议、服务器名、端口号、资源路径等信息，但不包括后面的查询参数部分。注意，getRequestURL()方法返回的结果是StringBuffer类型，而不是String类型，这样更便于对结果进行修改 |

### 1.2 获取客户机请求头

| 方法声明                            | 功能描述                                                     |
| ----------------------------------- | ------------------------------------------------------------ |
| String getHeader(String name)       | 该方法用于获取一个指定头字段的值，如果请求消息中没有包含指定的头字段，getHeader()方法返回null；如果请求消息中包含有多个指定名称的头字段，getHeader()方法返回其中第一个头字段的值 |
| Enumeration getHeaders(String name) | 该方法返回一个Enumeration集合对象，该集合对象由请求消息中出现的某个指定名称的所有头字段值组成。在多数情况下，一个头字段名在请求消息中只出现一次，但有时候可能会出现多次 |
| Enumeration getHeaderNames()        | 该方法用于获取一个包含所有请求头字段的Enumeration对象        |
| int getIntHeader(String name)       | 该方法用于获取指定名称的头字段，并且将其值转为int类型。需要注意的是，如果指定名称的头字段不存在，返回值为-1；如果获取到的头字段的值不能转为int类型，将发生NumberFormatException异常 |
| Long getDateHeader(String name)     | 该方法用于获取指定头字段的值，并将其按GMT时间格式转换成一个代表日期/时间的长整数，这个长整数是自1970年1月1日0点0分0秒算起的以毫秒为单位的时间值 |



### 1.3 获取请求参数

| 方法声明                                 | 功能描述                                                     |
| ---------------------------------------- | ------------------------------------------------------------ |
| String getParameter(String name)         | 该方法用于获取某个指定名称的参数值，如果请求消息中没有包含指定名称的参数，getParameter()方法返回null；如果指定名称的参数存在但没有设置值，则返回一个空串；如果请求消息中包含有多个该指定名称的参数，getParameter()方法返回第一个出现的参数值 |
| String[] getParameterValues(String name) | HTTP请求消息中可以有多个相同名称的参数（通常由一个包含有多个同名的字段元素的FORM表单生成），如果要获得HTTP请求消息中的同一个参数名所对应的所有参数值，那么就应该使用getParameterValues()方法，该方法用于返回一个String类型的数组 |
| Enumeration getParameterNames()          | getParameterNames()方法用于返回一个包含请求消息中所有参数名的Enumeration对象，在此基础上，可以对请求消息中的所有参数进行遍历处理 |
| Map getParameterMap()                    | getParameterMap()方法用于将请求消息中的所有参数名和值装入进一个Map对象中返回 |



## 二、HttpServletResponse

### 2.1 响应报文

当一个 Web 服务器响应一个 HTTP 请求时，响应通常包括一个状态行、一些响应报头、一个空行和文档。一个典型的响应如下所示：

```txt
HTTP/1.1 200 OK
Content-Type: text/html
Header2: ...
...
HeaderN: ...
  (Blank Line)
<!doctype ...>
<html>
<head>...</head>
<body>
...
</body>
</html>
```

状态行包括 HTTP 版本（在本例中为 HTTP/1.1）、一个状态码（在本例中为 200）和一个对应于状态码的短消息（在本例中为 OK）。



### 2.2 HttpServletResponse常用方法

| 方法 & 描述                                                  |
| :----------------------------------------------------------- |
| **String encodeRedirectURL(String url)** 为 sendRedirect 方法中使用的指定的 URL 进行编码，或者如果编码不是必需的，则返回 URL 未改变。 |
| **String encodeURL(String url)** 对包含 session 会话 ID 的指定 URL 进行编码，或者如果编码不是必需的，则返回 URL 未改变。 |
| **boolean containsHeader(String name)** 返回一个布尔值，指示是否已经设置已命名的响应报头。 |
| **boolean isCommitted()** 返回一个布尔值，指示响应是否已经提交。 |
| **void addCookie(Cookie cookie)** 把指定的 cookie 添加到响应。 |
| **void addDateHeader(String name, long date)** 添加一个带有给定的名称和日期值的响应报头。 |
| **void addHeader(String name, String value)** 添加一个带有给定的名称和值的响应报头。 |
| **void addIntHeader(String name, int value)** 添加一个带有给定的名称和整数值的响应报头。 |
| **void flushBuffer()** 强制任何在缓冲区中的内容被写入到客户端。 |
| **void reset()** 清除缓冲区中存在的任何数据，包括状态码和头。 |
| **void resetBuffer()** 清除响应中基础缓冲区的内容，不清除状态码和头。 |
| **void sendError(int sc)** 使用指定的状态码发送错误响应到客户端，并清除缓冲区。 |
| **void sendError(int sc, String msg)** 使用指定的状态发送错误响应到客户端。 |
| **void sendRedirect(String location)** 使用指定的重定向位置 URL 发送临时重定向响应到客户端。 |
| **void setBufferSize(int size)** 为响应主体设置首选的缓冲区大小。 |
| **void setCharacterEncoding(String charset)** 设置被发送到客户端的响应的字符编码（MIME 字符集）例如，UTF-8。 |
| **void setContentLength(int len)** 设置在 HTTP Servlet 响应中的内容主体的长度，该方法设置 HTTP Content-Length 头。 |
| **void setContentType(String type)** 如果响应还未被提交，设置被发送到客户端的响应的内容类型。 |
| **void setDateHeader(String name, long date)** 设置一个带有给定的名称和日期值的响应报头。 |
| **void setHeader(String name, String value)** 设置一个带有给定的名称和值的响应报头。 |
| **void setIntHeader(String name, int value)** 设置一个带有给定的名称和整数值的响应报头。 |
| **void setLocale(Locale loc)** 如果响应还未被提交，设置响应的区域。 |
| **void setStatus(int sc)** 为该响应设置状态码。              |

我们可以使用如下方法向客户端写出数据：

```java
PrintWriter out = response.getWriter();
out.println(...)
```



### 2.3 HTTP状态码

我们可以使用如下方法设置HTTP响应的状态码：

```java
response.setStatus(int status);
```

详细的HTTP状态码如下：

| 代码        | 消息                          | 描述                                                         |
| ----------- | ----------------------------- | ------------------------------------------------------------ |
| 100         | Continue                      | 只有请求的一部分已经被服务器接收，但只要它没有被拒绝，客户端应继续该请求。 |
| 101         | Switching Protocols           | 服务器切换协议。                                             |
| **==200==** | **==OK==**                    | **==请求成功。==**                                           |
| 201         | Created                       | 该请求是完整的，并创建一个新的资源。                         |
| 202         | Accepted                      | 该请求被接受处理，但是该处理是不完整的。                     |
| 203         | Non-authoritative Information |                                                              |
| 204         | No Content                    |                                                              |
| 205         | Reset Content                 |                                                              |
| 206         | Partial Content               |                                                              |
| 300         | Multiple Choices              | 链接列表。用户可以选择一个链接，进入到该位置。最多五个地址。 |
| **==301==** | **==Moved Permanently==**     | **==所请求的页面已经转移到一个新的 URL。==**                 |
| 302         | Found                         | 所请求的页面已经临时转移到一个新的 URL。                     |
| 303         | See Other                     | 所请求的页面可以在另一个不同的 URL 下被找到。                |
| 304         | Not Modified                  |                                                              |
| 305         | Use Proxy                     |                                                              |
| 306         | *Unused*                      | 在以前的版本中使用该代码。现在已不再使用它，但代码仍被保留。 |
| 307         | Temporary Redirect            | 所请求的页面已经临时转移到一个新的 URL。                     |
| 400         | Bad Request                   | 服务器不理解请求。                                           |
| 401         | Unauthorized                  | 所请求的页面需要用户名和密码。                               |
| 402         | Payment Required              | *您还不能使用该代码。*                                       |
| 403         | Forbidden                     | 禁止访问所请求的页面。                                       |
| **==404==** | **==Not Found==**             | **==服务器无法找到所请求的页面。==**                         |
| 405         | Method Not Allowed            | 在请求中指定的方法是不允许的。                               |
| 406         | Not Acceptable                | 服务器只生成一个不被客户端接受的响应。                       |
| 407         | Proxy Authentication Required | 在请求送达之前，您必须使用代理服务器的验证。                 |
| 408         | Request Timeout               | 请求需要的时间比服务器能够等待的时间长，超时。               |
| 409         | Conflict                      | 请求因为冲突无法完成。                                       |
| 410         | Gone                          | 所请求的页面不再可用。                                       |
| 411         | Length Required               | "Content-Length" 未定义。服务器无法处理客户端发送的不带 Content-Length 的请求信息。 |
| 412         | Precondition Failed           | 请求中给出的先决条件被服务器评估为 false。                   |
| 413         | Request Entity Too Large      | 服务器不接受该请求，因为请求实体过大。                       |
| 414         | Request-url Too Long          | 服务器不接受该请求，因为 URL 太长。当您转换一个 "post" 请求为一个带有长的查询信息的 "get" 请求时发生。 |
| 415         | Unsupported Media Type        | 服务器不接受该请求，因为媒体类型不被支持。                   |
| 417         | Expectation Failed            |                                                              |
| 500         | Internal Server Error         | 未完成的请求。服务器遇到了一个意外的情况。                   |
| 501         | Not Implemented               | 未完成的请求。服务器不支持所需的功能。                       |
| 502         | Bad Gateway                   | 未完成的请求。服务器从上游服务器收到无效响应。               |
| 503         | Service Unavailable           | 未完成的请求。服务器暂时超载或死机。                         |
| 504         | Gateway Timeout               | 网关超时。                                                   |
| 505         | HTTP Version Not Supported    | 服务器不支持"HTTP协议"版本。                                 |