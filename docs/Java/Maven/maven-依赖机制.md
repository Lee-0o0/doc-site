# Maven— —依赖机制

本文主要介绍Maven中的依赖机制。



## 1. 引入依赖体会

### 1.1 引入依赖的原始做法

现在要求我们的项目连接MySQL数据库，需要怎么做呢？

首先连接MySQL数据库需要引入以下jar包(版本号省略)：

```txt
mysql-connector-java-xxx.jar
```

首先，存在一个问题：去哪下载jar包？

然后将jar放入项目下的lib目录，并且`Add as Library`：

![image-20210512201432371](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/2378a494c141d2a0d030ee00f71bf17f--03a3--image-20210512201432371.png)

之后才能使用。这个过程稍显麻烦。



### 1.2 引入依赖的maven做法

如果我们使用maven管理我们的项目，那么当我们的项目想要连接数据库，引入依赖时，只需要在pom.xml文件中添加下面的内容：

```xml
<dependencies>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.20</version>
    </dependency>
</dependencies>
```

之后，我们就可以在项目中使用jdbc连接MySQL数据库了。

此时，我们可以解答上一小节的问题，去哪下载jar包？引入maven后，问题变为去哪找到这一串XML代码？

答案很简单，去maven仓库就可以找到相关的依赖项：https://mvnrepository.com/

例如：

![image-20210512213220641](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/111796d8fca88736f20d689917d22b58--02a2--image-20210512213220641.png)

引入依赖后，在IDEA中显示如下：

![image-20210512211624040](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/e13907e40e15583d980ff85ca9c4ff13--34e4--image-20210512211624040.png)



## 2. 依赖的作用域

依赖作用域用于限制依赖项的传递性，并确定什么时候将依赖包含在类路径中（项目）。在pom.xml文件中，通过\<dependency\>元素中的标签`<scope>`指定。

首先明确maven项目的生命周期，大致可以分为这么几个阶段 **编译--》测试--》打包--》运行** 。

存在六种依赖作用域：**compile**、**provided**、**test**、**runtime**、**system**、**import**

最常用的作用域是compile、provided和test。在看具体的作用讲解前，先明确几个概念：

- main目录：src\main目录，存放程序源代码
- test目录：src\test目录，存放程序测试代码
- 开发过程：即我们写代码时候的项目
- 部署到服务器：即打包后的项目，如jar包

开发过程是否有效，意思是在我们的项目中引入了一个依赖A，我们能不能在写代码时使用A中的类，如果能则开发过程有效；

部署到服务器是否有效，意思是在我们的项目中引入了一个依赖A，当我们的项目打包时，依赖A是否也一起被打包，如果是则部署到服务器有效；

**①compile 和 test 对比**

|         | main目录（空间） | test目录（空间） | 开发过程（时间） | 部署到服务器（时间） |
| ------- | ---------------- | ---------------- | ---------------- | -------------------- |
| compile | 有效             | 有效             | 有效             | 有效                 |
| test    | 无效             | 有效             | 有效             | 无效                 |

**②compile 和 provided 对比**

|          | main目录（空间） | test目录（空间） | 开发过程（时间） | 部署到服务器（时间） |
| -------- | ---------------- | ---------------- | ---------------- | -------------------- |
| compile  | 有效             | 有效             | 有效             | 有效                 |
| provided | 有效             | 有效             | 有效             | 无效                 |

例如一个web项目(打成war包)，我们就可以将`javax.servlet-api`依赖设置成`provided`，因为Tomcat已经提供了`javax.servlet-api`，如果我们还将`javax.servlet-api`打进war包中，那就会造成重复、冲突。

```xml
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>javax.servlet-api</artifactId>
	<version>3.1.0</version>
	<scope>provided</scope>
</dependency>
```



**import范围**

管理依赖最基本的办法是继承父工程，但是和 Java 类一样，Maven 也是单继承的。如果不同体系的依赖信息封装在不同 POM 中了，没办法继承多个父工程怎么办？这时就可以使用 import 依赖范围。

典型案例当然是在项目中引入 SpringBoot、SpringCloud 依赖：

```xml
<dependencyManagement>
    <dependencies>

        <!-- SpringCloud 依赖导入 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Hoxton.SR9</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>

        <!-- SpringCloud Alibaba 依赖导入 -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2.2.6.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>

        <!-- SpringBoot 依赖导入 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>2.3.6.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

import 依赖范围使用要求：

- 打包类型必须是 pom
- 必须放在 dependencyManagement 中

**system范围**

以 Windows 系统环境下开发为例，假设现在 D:\tempare\atguigu-maven-test-aaa-1.0-SNAPSHOT.jar 想要引入到我们的项目中，此时我们就可以将依赖配置为 system 范围：

```xml
<dependency>
    <groupId>com.atguigu.maven</groupId>
    <artifactId>atguigu-maven-test-aaa</artifactId>
    <version>1.0-SNAPSHOT</version>
    <systemPath>D:\tempare\atguigu-maven-test-aaa-1.0-SNAPSHOT.jar</systemPath>
    <scope>system</scope>
</dependency>
```

**runtime范围**

专门用于编译时不需要，但是运行时需要的 jar 包。比如：编译时我们根据接口调用方法，但是实际运行时需要的是接口的实现类。典型案例是：

```java
<!--热部署 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>
```



## 3. 依赖的传递性

所谓依赖的传递性，是指如果一个项目A依赖项目B，而项目B又依赖项目C，那么项目A也会引入依赖C。

传递原则：在 A 依赖 B，B 依赖 C 的前提下，C 是否能够传递到 A，取决于 B 依赖 C 时使用的依赖范围。

- B 依赖 C 时使用 compile 范围：可以传递
- B 依赖 C 时使用 test 或 provided 范围：不能传递，所以需要这样的 jar 包时，就必须在需要的地方明确配置依赖才可以。

例如：

![image-20210513131214897](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/b1298a32ede40992140da315fa298633--2660--image-20210513131214897.png)

在我们的项目中，只引入了两个依赖：`junit`和`mysql-connector-java`，但实际引入了四个依赖，还包括`junit`依赖的`hamcrest-core`，`mysql-connector-java`依赖的`protobuf-java`。

注意：`junit`依赖`hamcrest-core`时，范围是`compile`的，所以`hamcrest-core`传递到了我们的项目中。

```xml
<!-- 取自 junit pom文件 -->
<dependencies>
    <dependency>
        <groupId>org.hamcrest</groupId>
        <artifactId>hamcrest-core</artifactId>
        <version>1.3</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```



**依赖仲裁**

依赖仲裁是指如果出现某个依赖，存在不同版本，选择哪个依赖的问题。

如下，项目A的两条依赖路径为A--》B--》C--》D 2.0和A--》E--》D 1.0，同时依赖了D，但是版本不同，此时A应该引入哪个版本的依赖呢？是D 1.0还是D 2.0呢？

```txt
  A
  ├── B
  │   └── C
  │       └── D 2.0
  └── E
      └── D 1.0
```

maven采用**就近原则**进行选择，是指在依赖树中，哪个版本的依赖离根项目最近，就选择哪个版本。在上述例子中，D 1.0比D 2.0到A的路径短，则选择依赖D 1.0。

注意，如果两个依赖版本在依赖树中具有相同的深度，那么第一个声明的依赖将获胜。

当然，我们也可以在项目中明确指定版本，这样A就依赖D 2.0了：

```txt
  A
  ├── B
  │   └── C
  │       └── D 2.0
  ├── E
  │   └── D 1.0
  │
  └── D 2.0   
```

**移除依赖**

我们也可以移除某一个依赖的依赖，来使得只依赖某一个版本的依赖。移除依赖可以通过`<exclusion>`实现。例如：

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.11</version>
        <scope>test</scope>
        <exclusions>
            <exclusion>
                <groupId>org.hamcrest</groupId>
                <artifactId>hamcrest-core</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

![image-20210514194512798](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/cb74085dfb6100cc68a9f1e7ad76cb26--c398--image-20210514194512798.png)



## 4. 依赖版本管理

Maven工程可以继承，例如：A 工程继承 B 工程

- B 工程：父工程
- A 工程：子工程

本质上是 A 工程的 pom.xml 中的配置继承了 B 工程中 pom.xml 的配置。继承的作用之一是在父工程中统一管理项目中的依赖信息，具体来说是管理依赖信息的版本。

### 4.1 创建父工程

创建父工程后，修改pom.xml文件：

```xml
<groupId>com.lee.maven</groupId>
<artifactId>maven-parent</artifactId>
<version>1.0-SNAPSHOT</version>

<!-- 当前工程作为父工程，它要去管理子工程，所以打包方式必须是 pom -->
<packaging>pom</packaging>
```

然后在父工程中管理依赖版本：

```xml
<!-- 使用dependencyManagement标签配置对依赖的管理 -->
<!-- 被管理的依赖并没有真正被引入到工程 -->
<dependencyManagement>
	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-beans</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-expression</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
			<version>4.0.0.RELEASE</version>
		</dependency>
	</dependencies>
</dependencyManagement>
```

### 4.2 创建子工程

创建子工程后，pom.xml文件：

```xml
<!-- 使用parent标签指定当前工程的父工程 -->
<parent>
	<!-- 父工程的坐标 -->
	<groupId>com.lee.maven</groupId>
	<artifactId>maven-parent</artifactId>
	<version>1.0-SNAPSHOT</version>
</parent>

<!-- 子工程的坐标 -->
<!-- 如果子工程坐标中的groupId和version与父工程一致，那么可以省略 -->
<!-- <groupId>com.lee.maven</groupId> -->
<artifactId>maven-child</artifactId>
<!-- <version>1.0-SNAPSHOT</version> -->
```

此时父工程中的pom.xml中应该自动添加了`<modules>`标签，用于指定子模块：

```xml
<modules>
    <module>maven-child</module>
</modules>
```

然后我们在子工程中声明依赖时，就可以省略版本号了（由父工程的dependencyManagement来决定）：

```xml
<!-- 子工程引用父工程中的依赖信息时，可以把版本号去掉。	-->
<!-- 把版本号去掉就表示子工程中这个依赖的版本由父工程决定。 -->
<!-- 具体来说是由父工程的dependencyManagement来决定。 -->
<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-core</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-beans</artifactId>
	</dependency>
</dependencies>
```



## 5. 仓库

仓库是指存放依赖的地方，分为本地仓库和远程仓库。

- 本地仓库：本地仓库是maven可运行的计算机上的一个目录，它缓存远程下载，并包含您尚未发布的临时构件（通俗来说，自己构建的jar包）。
- 远程仓库：远程仓库是指任何其他类型（即除了本地仓库外，其它仓库都属于远程仓库）的仓库，可以通过各种协议（例如 `file://`和`https://`）进行访问。 这些仓库可能是由第三方建立的，提供构件供下载（例如`repo.maven.apache.org`）。 其他远程仓库可能是在公司内的文件或HTTP服务器上设置的内部仓库，用于在开发团队之间共享私有的构件以及用于发布，这也称为私有仓库。

**本地仓库**

默认情况下（Windows环境中），本地仓库的地址是`${user.home}\.m2\repository`，如果我们不做任何配置，当然可以使用，但是在我们项目开发中，会不断压缩C盘的空间，所以我们可以修改maven的配置文件，更改本地仓库的位置。

首先找到maven安装目录下的`conf\settings.xml`文件，然后修改`<localRepository>`元素的值：

![image-20210514211112238](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-01/0ba0d307423b5ea0394d731501323535--1ca8--image-20210514211112238.png)

**远程仓库**

由第三方搭建的公共仓库，例如：https://repo.maven.apache.org/maven2/

**私有仓库**

我们可以使用nexus搭建公司内部的仓库，称为私有仓库。



## 参考资料

[1] 尚硅谷Maven视频教程

[2] http://heavy_code_industry.gitee.io/code_heavy_industry/