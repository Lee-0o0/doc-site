# Typora— —图床搭建教程

究其根本，Typora只是一个Markdown文件编辑工具，Markdown只是一个文本格式文件。换句话说，Markdown就像一个txt文档，只能包含字符。这不对呀！Markdown文档里不是还有图片吗？这是因为Markdown包含有指向这些图片的链接，Markdown文件并不拥有这些图片。如果我们在Markdown中以相对路径引用图片，这就会造成移动Markdown文件时，因为引用不到图片，而造成图片丢失的情况。所以，最好的解决方法是将图片保存在图床中，然后在Markdown中引用图床中的图片。

在本教程中，我将会教大家搭建 **Typora+PicGo+jsDeliver+GitHub** 的图床系统。



## 1. GitHub设置

首先在GitHub中创建一个图床仓库，并将仓库的可见性设置为`public`：

![image-20210423200031525](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-04-23/9e485515f01a59ee910e2544aed78694--b72d--9e485515f01a59ee910e2544aed78694--7d8d--image-20210423200031525.png)

然后创建Token，点击链接：https://github.com/settings/tokens进入Token创建页面：

![image-20210423200324692](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-04-23/cfa54ece0ca67d4f70fe67a43ba05e3b--ae40--cfa54ece0ca67d4f70fe67a43ba05e3b--99d9--image-20210423200324692.png)

然后点击`Generate new token`进行Token创建：

![image-20210423200533420](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-04-23/1acc21049561ae189b49e4652b25af6c--a014--1acc21049561ae189b49e4652b25af6c--e65b--image-20210423200533420.png)

**完成后不要着急离开或关闭页面，复制生成的Token，保存到文本中！**

![image-20210423200730181](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-04-23/7ef7345e43d2ddcc7a569fbd800c0607--977d--7ef7345e43d2ddcc7a569fbd800c0607--2495--image-20210423200730181.png)

至此，GitHub的工作已完成。



## 2.  下载PicGo

首先下载PicGo，下载地址：https://molunerfinn.com/PicGo/，安装后记住PicGo的安装目录。

然后进行PicGo的设置，首先打开PicGo，然后选择图床设置--》GitHub图床：

![image-20210423200928995](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-04-23/ce588d1ea78351c5450de5ad58f083c8--8b3a--ce588d1ea78351c5450de5ad58f083c8--9157--image-20210423200928995.png)

详细设置项如下：

- 设定仓库名，设置规则为`your-github-username/image-repo`，即你的`GitHub帐户名/图床仓库名`；
- 设定分支名，默认为`master`就好；
- 设定Token：就是刚刚保存的Token，复制粘贴进这里就好；
- 指定存储路径：可以设置图片存储在仓库的哪个目录下，这里就设置为图片存储在仓库的`PicGo`目录；
- 设定自定义域名：为了使用jsdeliver进行加速，此处设置规则为：`https://cdn.jsdeliver.net/GitHub帐户名/图床仓库名`；

设置完成后，点击确定，此时PicGo设置完成。



## 3. Typora设置

首先打开Typora的设置面板（文件 — — 偏好设置），找到图像一栏，设置如下：

![image-20210423201751150](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-04-23/de7c72f4f6c2124a0d2311a6a3f7b25f--7e7a--de7c72f4f6c2124a0d2311a6a3f7b25f--7b51--image-20210423201751150.png)

验证结果如下：

![image-20210423201823707](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-04-23/307d08717d68c728ad399cafce6e88c0--8697--307d08717d68c728ad399cafce6e88c0--ebc7--image-20210423201823707.png)

此后，只要我们在Typora中写文章时，当我们插入图片时，Typora就会自动调用PicGo程序，帮我们将图片上传到GitHub中去，然后将返回的图片地址进行替换，这样我们文章中的图片地址就是GitHub中的图片地址了，以后文章迁移就更加方便了。

此时您可以看看GitHub仓库中是否有图片了。



## 4. 其它

### 4.1 安装PicGo插件

如果我们只是简单地使用PicGo进行图片上传，那么我们无法上传同名的图片，所以我们需要安装插件，对图片进行重命名。在PicGo中选择插件设置，搜索`rename-file`进行安装：

![image-20210423202243222](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-04-23/105aa90b194e0beb35982199722ded68--63ff--105aa90b194e0beb35982199722ded68--6aa8--image-20210423202243222.png)

鼠标右键单击插件齿轮，选择`配置plugin：picgo-plugin-rename-file`，配置项如下：

![image-20210423202326447](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-04-23/bf8ad358c3e7f02a28c8e837d7a3a138--8e7e--bf8ad358c3e7f02a28c8e837d7a3a138--4593--image-20210423202326447.png)

详细配置请查看参考资料[\[3]](https://github.com/liuwave/picgo-plugin-rename-file).



### 4.2 PicGo-Server设置项

该项设置是指可以将PicGo当作一个服务器，然后通过`127.0.0.1:36677`调用上传图片的API接口，这也是Typora的实现原理，所以**一定不要将这项设置关闭了！**否则Typora无法成功调用PicGo的服务，进行图片上传。

![image-20210423202734386](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-04-23/a3774893f6b5e4fde5e3eb883070c814--06c1--a3774893f6b5e4fde5e3eb883070c814--b67e--image-20210423202734386.png)



### 4.3 文章建议

虽然经过以上设置，我们成功搭建了图床，使得Typora成功上传了图片，但是我还是建议将Typora的图像偏好设置为如下：

![image-20210423203104666](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2021-04-23/fad0a2eae9b67db798097e66a88c6dd5--003b--fad0a2eae9b67db798097e66a88c6dd5--bb57--image-20210423203104666.png)

即还是在本地中保存图片，然后当你的文章写完后，再通过**格式—》图像—》上传所有本地图片**，将文章中的图片一次性上传。这样我们在本地也保存了一份图片，更加保险；而且文章写完后再将所有的图片上传，这样GitHub保存的图片都是有用的，不至于让无用的图片占据图床仓库空间。



## 参考资料

[1] Typora官方教程：https://support.typora.io/Upload-Image/

[2] PicGo官方教程：https://github.com/Molunerfinn/PicGo

[3] rename-file教程：https://github.com/liuwave/picgo-plugin-rename-file

[4] jsdeliver官网：https://www.jsdelivr.com/

[5] 使用jsDeliver加速：https://www.jianshu.com/p/2097bef17cbe