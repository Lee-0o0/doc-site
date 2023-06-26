# RDLC-概述

## 1. 报表文件

报表文件是用于渲染来自多个数据源数据的布局描述文件，与报表系统绑定。报表系统提供了特定的元素，用于创建报表内容，常见的报表元素包括文本标签、数据表、图标以及其他可视元素。

报表文件有以下几种：RDL、RDLC、RPT。

### 1.1 什么是RDL

RDL（Report Definition Language）是由微软定义的用于设计报表的基准集，由XML语言描述。RDL文件可以包含一个或多个报表元素，报表元素有简单和复杂之分，简单的元素不包含子元素和属性，复杂的元素有子元素和可选的属性。

我们可以从零开始创建RDL文件，只要遵循RDL模式定义（RDL XML Schema Definition，XSD），但是，为了简单起见，我们可以借助软件帮我们创建RDL文件，例如[Report Builder](https://learn.microsoft.com/en-us/sql/reporting-services/install-windows/install-report-builder?view=sql-server-ver16#download)。



### 1.2 什么是RDLC

RDLC是RDL的扩展，用于客户端，全称为Report Definition Language Client-side。我们可以通过在Visual Studio中安装插件来创建RDLC报表文件，ReportViewer控件可用于客户端执行显示报表。



### 1.3 什么是RPT

后缀名为.RPT的报表是水晶报表文件（Crystal Reports File），在世界范围内有大量使用者，主要用于小中型企业。



## 2. RDLC开发环境搭建

IDE：Visual Studio Community 2019（以下简称VS2019）

### 2.1 安装插件

在VS2019中选择扩展-->管理扩展，打开如下界面，选择联机页。

- 在右上角输入框中输入rdlc
- 选择Microsoft RDLC Report Designer并下载安装
- 重启VS2019

![image-20230626102337255](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-26/b9b107a227ce52d74ced6224381049a2-da44-image-20230626102337255.png)



### 2.2 新建报表

在我们的项目上，右键选择添加-->新建项，找到报表项：

![image-20230626102855954](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-26/31e7990e78045d8be4e7bc1e0d5990fb-d9ae-image-20230626102855954.png)

一般情况下，我们选择新建空白报表。创建完成后页面如下：

- 左边是报表数据，存放报表要显示的数据
- 中间是报表设计界面
- 右边是工具箱，是报表元素存放位置

![image-20230626103036740](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-26/c960c7ea985d83ceecffc9af0c296019-5ab7-image-20230626103036740.png)

从工具箱中拖动一个文本框到报表中，然后改变文本框的字体为宋体，输入”第一个报表“字样：

![image-20230626104013746](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-26/afd8d988e0f12970feafb08194906c2a-7bd5-image-20230626104013746.png)

### 2.3 安装ReportViewer

为了引入ReportViewer，需要在项目中引入相关依赖：

- 打开NuGet依赖管理界面，选择浏览页
- 输入ReportViewerControl关键搜索
- 选择Microsoft.ReportingServices.ReportViewerControl.Winforms
- 选择合适的版本并安装

![image-20230626103713485](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-26/91fecae6edd519c88c1d18a2f9955855-89e8-image-20230626103713485.png)

### 2.4 设计页面并启动

完成后，我们就可以在页面（Form）的工具箱中找到ReportViewer，拖动到页面中，设置属性：

![image-20230626104336433](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-26/6c41e3b1f5e43d992144b7d47faee147-725f-image-20230626104336433.png)

启动项目，报表就可以显示在页面上了：

![image-20230626104455607](https://raw.githubusercontent.com/Lee-0o0/image-store/master/PicGo/2023-06-26/809eea96a87a900d0f7bb7eb36ecc30f-1c32-image-20230626104455607.png)