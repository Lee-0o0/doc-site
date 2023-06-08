# Maven— —Maven介绍、安装与入门

本文主要介绍Maven、安装方法及第一个Maven程序。



## 1. maven介绍

> Apache Maven is a software project management and comprehension tool. Based on the concept of a project object model (POM), Maven can manage a project's build, reporting and documentation from a central piece of information.

简而言之，maven是一个项目管理工具，帮助我们更好地构建项目。

maven可以帮助我们管理项目依赖和构建项目。

- 管理项目依赖：项目依赖管理主要做以下事情：
  - jar 包的下载：使用 Maven 之后，jar 包会从规范的远程仓库下载到本地
  - jar 包之间的依赖：通过依赖的传递性自动完成
  - jar 包之间的冲突：通过对依赖的配置进行调整，让某些jar包不会被导入
- 构建项目：将我们编写的源代码构建成可部署运行的程序。构建过程包含的主要的环节：
  - 清理：删除上一次构建的结果，为下一次构建做好准备
  - 编译：Java 源程序编译成 *.class 字节码文件
  - 测试：运行提前准备好的测试程序
  - 报告：针对刚才测试的结果生成一个全面的信息
  - 打包
    - Java工程：jar包
    - Web工程：war包
  - 安装：把一个 Maven 工程经过打包操作生成的 jar 包或 war 包存入 Maven 仓库
  - 部署
    - 部署 jar 包：把一个 jar 包部署到 Nexus 私服服务器上
    - 部署 war 包：借助相关 Maven 插件（例如 cargo），将 war 包部署到 Tomcat 服务器上




## 2. maven安装

首先下载maven，下载页面：http://maven.apache.org/download.cgi，根据不同平台选择下载版本：

![image-20210511135924997](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/d1273608da856563f05b16bad6545d12--99a4--image-20210511135924997.png)

下载完成后解压，目录结构如下：

![image-20210511140201764](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/62027a22478f6538b6d54926fd33a7a8--1cf5--image-20210511140201764.png)

在cmd环境下进入bin目录，输入`mvn-v`查看版本：

![image-20210511140245438](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/7003e889e9f05a80fcb54212fdbcccb2--4433--image-20210511140245438.png)

当然，如果每次使用maven都需要进入安装目录中，这会很麻烦，所以我们将其配置到系统变量中。

在path变量中添加maven的bin目录地址：（此处是我之前安装的maven，所以版本是3.6.3）

![image-20210511140439558](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/db2474575e7bc8918a62183f3cb25f08--740e--image-20210511140439558.png)

这样cmd在任意的路径下都可以执行maven命令：

![image-20210511140626813](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/d5ec9ef78c10182a8795e36f0abb2cc2--7c88--image-20210511140626813.png)



## 3. 创建第一个项目

在cmd下运行如下命令，创建第一个项目：

```shell
mvn archetype:generate -DgroupId=com.lee -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false -DarchetypeCatalog=internal
```

**注意：**选项`-DarchetypeCatalog=internal`可以删去。

![image-20210511141549617](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/c0e6af23648d25e068a193cc7e3140a1--bb6c--image-20210511141549617.png)

提示已在桌面上创建了一个项目。我们查看这个新项目的目录结构：

![image-20210511141712327](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/17c42540c20ce008a1eecea48d791553--5100--image-20210511141712327.png)

上述命令自带了一些参数，我们也可以执行下面的命令：

```shell
mvn archetype:generate
```

然后在控制台中输入相关参数。

可以发现，maven帮助我们创建了基础的java项目包、类等文件，并且，最重要的是还有一个pom.xml文件，该文件内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.lee</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>my-app</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
        <!-- site lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#site_Lifecycle -->
        <plugin>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.7.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>3.0.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```

**==pom.xml是maven管理我们项目的核心！！！==**

当创建新项目后，需要对我们的项目进行编译。只需要进入my-app目录，在cmd中运行以下命令：

```shell
mvn compile
```

结果会在my-app目录下新建target目录，项目整体的目录结构如下：

![image-20210511142619529](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/4060a240647efa2b8e22d0de69693059--a088--image-20210511142619529.png)

我们可以进入 `my-app\target\classes`下，利用Java 命令执行App.class文件：

```shell
java com.lee.App
```

![image-20210511144014510](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/7dfa44a0d282c0361583d1c68c96e1f4--bea3--image-20210511144014510.png)

成功输出`Hello World!`。

如果我们想把项目达成一个包，只需要在my-app目录下运行如下命令：

```shell
mvn package
```

![image-20210511144241468](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/20b697fcfb578a25d5a50c29ac57275f--43e6--image-20210511144241468.png)

在target目录下成功生成了jar 包，我们进入target 目录，运行以下命令：

```shell
java -jar my-app-1.0-SNAPSHOT.jar
```

但是并没有成功运行：

![image-20210511144730047](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/7ac3009b36279a98b2fe2d9c9c494b8e--f9a5--image-20210511144730047.png)

原因何在？

首先我们进入jar包，找到MANIFEST.MF文件，其内容如下：

```txt
Manifest-Version: 1.0
Created-By: Apache Maven 3.6.3
Built-By: Administrator
Build-Jdk: 11
```

其中缺少了一项主类的配置：

```txt
Main-Class: xx.xx.xx
```

因此，我们可以自己手动加上主类配置，但我们还是用maven的方式解决吧。修改pom.xml文件，如下在maven-jar-plugin插件中添加配置项：

```xml
<build>
    <plugins>
        <plugin>
            <artifactId>maven-jar-plugin</artifactId>
            <version>3.0.2</version>
            <configuration>
                <archive>
                    <manifest>
                        <mainClass>com.lee.App</mainClass>
                    </manifest>
                </archive>						
            </configuration>
        </plugin>
    </plugins>
</build>
```

然后重新打包，运行，结果如下：

![image-20210511150338615](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/d021383a4aaeeee3a5e6add5b226b584--97c3--image-20210511150338615.png)

此时MANIFEST.MF文件内容如下：

```txt
Manifest-Version: 1.0
Created-By: Apache Maven 3.6.3
Built-By: Administrator
Build-Jdk: 11
Main-Class: com.lee.App
```

我们可以使用如下命令删除target文件夹：

```shell
mvn clean
```

![image-20210511150726526](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/fd1b33f261e4c62b61d95dd9c2dbc103--e447--image-20210511150726526.png)



## 参考资料

[1] maven官网：http://maven.apache.org/

[2] maven打包缺少主类配置项：

问题明确：https://blog.csdn.net/xqnode/article/details/86628794

问题解决：https://blog.csdn.net/zwc2xm/article/details/91492284

[3] http://heavy_code_industry.gitee.io/code_heavy_industry/pro002-maven/

