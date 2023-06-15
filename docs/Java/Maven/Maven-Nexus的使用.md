# Maven— —Nexus的使用

## 1. Nexus介绍

Nexus 是 Sonatype 公司发布的一款仓库（Repository）管理软件，常用来搭建 Maven 私服，所以也有人将 Nexus 称为“Maven仓库管理器”。

能够帮助我们建立私服的软件被称为 Maven 仓库管理器，主要有以下 3 种：

- Apache Archiva

- JFrog Artifactory

- Sonatype Nexus

其中，Sonatype Nexus 是当前最流行，使用最广泛的 Maven 仓库管理器。Nexus 分为开源版和专业版，其中开源版足以满足大部分 Maven 用户的需求。



## 2. 下载与启动

下载链接：https://help.sonatype.com/repomanager3/product-information/download

根据自己需要，下载不同操作系统对应的Nexus，此处为演示，下载Windows版本：

![image-20230614111207327](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-15/bdaffa9cfdd344599c652f507f9fc89d-46f3-image-20230614111207327.png)

下载完成后，解压缩，进入bin目录：

![image-20230614111400308](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-15/ceecbf0c31d5402672866f43abad7965-1d57-image-20230614111400308.png)

在该目录下打开命令行窗口，执行如下命令，启动Nexus：

```sh
nexus.exe /run
```

稍等一会儿后，浏览器访问`http://localhost:8081/`，成功打开Nexus界面：

![image-20230614111605336](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-15/4a15930ae19ab33fb5aa866ee7244716-8ea3-image-20230614111605336.png)



## 3. 登录与初始化

点击右上角`Sign in`按钮，进行登录，初次登录会提示我们admin账号的密码所在位置：

![image-20230614111655007](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-15/e0e56803edaeeb08dd96396eda8a65c2-a438-image-20230614111655007.png)

初次登录成功后会让我们重置密码，演示用重置密码为admin：

![image-20230614111836113](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-15/fa31644d83ce43a7d04e8421b3946e55-ae8e-image-20230614111836113.png)

然后进行匿名访问配置，此处选择禁止匿名访问：

![image-20230614111918284](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-15/90ff90a43e042f6a9b5f22a37a0aac88-f1c0-image-20230614111918284.png)

至此，登录与初始化结束，之后我们就可以配置与使用Nexus了。



## 4. Nexus仓库介绍

![image-20230614144510536](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-15/e24e1b96f83a6803d55b1bf22240cbd8-84af-image-20230614144510536.png)

### 4.1 仓库类型

在Nexus中，有三种仓库类型：

- proxy：代理仓库，用来代理远程公共仓库，如 Maven 中央仓库；
- hosted：宿主仓库，是Nexus的本地仓库，用来存放本地项目所产生的构件；
- group：仓库组，用来聚合代理仓库和宿主仓库，为这些仓库提供统一的服务地址，以便 Maven 可以更加方便地获得这些仓库中的构件。是一个概念性仓库，并不真正存放构件；

### 4.2 设置代理仓库

我们可以在仓库的设置界面中设置代理仓库的远程仓库：

![image-20230615085156159](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-15/bddbadbcbf9ab097631bb0d2e1bb8e35-38db-image-20230615085156159.png)

设置后保存即可。

### 4.3 设置仓库组

我们可以设置仓库的成员（即聚合了哪些仓库）：

![image-20230615085327452](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-15/0b5c418ddefef205ce09ad35e5d7664a-5a30-image-20230615085327452.png)

### 4.4 手动上传构件

准备一个Jar包：test-1.0.jar，然后我们手动将这个Jar包上传到Nexus中。

![image-20230615155959097](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-15/bd718bfa26fc9913b51352cb02e92fd4-6daa-image-20230615155959097.png)



## 5. 配置Maven

### 5.1 获取构件

现在我们就可以配置Maven以使用Nexus中的Jar包。

首先在settings.xml中配置`<profile>`，新加Nexus仓库：

```xml
<profiles>
    <profile>
        <id>my-nexus-profile</id>
        <repositories>
            <repository>
                <id>my-nexus-repo</id>
                <url>http://localhost:8081/repository/maven-public/</url>
                <releases>
                    <enabled>true</enabled>
                </releases>
                <snapshots>
                    <enabled>true</enabled>
                </snapshots>
            </repository>
        </repositories>
    </profile>
</profiles>

<activeProfiles>
    <activeProfile>my-nexus-profile</activeProfile>
</activeProfiles>
```

然后在settings.xml中配置账号和密码：

```xml
<servers>
    <server>
        <id>my-nexus-repo</id>
        <username>admin</username>
        <password>admin</password>
    </server>
</servers>
```

**注意**：`server`中的`id`要与`repository`的`id`一致。

现在就可以在我们的项目中使用Nexus了，例如，在项目中引入刚刚我们手动上传的Jar包：

```xml
<dependencies>
    <dependency>
        <groupId>com.lee.nexus</groupId>
        <artifactId>test</artifactId>
        <version>1.0</version>
    </dependency>
</dependencies>
```



### 5.2 上传构件

在我们的项目pom.xml中配置如下信息：

```xml
<distributionManagement>
    <!-- maven-releases 仓库 -->
    <repository>
        <id>my-nexus-releases-repo</id>
        <url>http://localhost:8081/repository/maven-releases/</url>
    </repository>
    <!-- maven-snapshots 仓库 -->
    <snapshotRepository>
        <id>my-nexus-snapshots-repo</id>
        <url>http://localhost:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```

注意：由于上传构件也是需要账号密码的，所以我们要新增两个`<server>`配置，注意`id`与`repository`和`snapshotRepository`的`id`相对应。

```xml
<servers>
    <server>
        <id>my-nexus-releases-repo</id>
        <username>admin</username>
        <password>admin</password>
    </server>
    <server>
        <id>my-nexus-snapshots-repo</id>
        <username>admin</username>
        <password>admin</password>
    </server>
</servers>
```

然后，运行`deploy`生命周期即可部署我们的构件到Nexus仓库中。

![image-20230615165215063](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-15/c11a4b56e15f914ab9af1996d0afb517-c04f-image-20230615165215063.png)
