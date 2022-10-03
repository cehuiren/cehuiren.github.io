---
layout: post
title:  "MicroStation Plan Section Callout剖断面并提取断面线"
date: 2022-06-03
categories: MicroStation 剖切
tags: MicroStation 剖面 断面
excerpt: MicroStation可使用Plan/Section Callout工具剖切横断面，但生成的图纸对象是与原模型动态关联的，不能直接作为断面线使用，为了进一步处理模断面用于面积、方量计算，需要将剖切面的断面线对象提取出来，本文详细说明了提取断面线的过程。
author: 测量老覃
---
* content
{:toc}

### 创建剖切对象模型 

使用Mesh From Point工具生成Mesh元素或其它建模工具生成需剖切的三维模型。

![](/img/2022/2022-09-04-14-57-16.png)

### 在顶视图内剖切

移到Top View，其它方向剖切需先转换到相应（或垂直）的视图方向。

### 放置剖切面 

使用Drawing Composition〉Place Pan Callout工具，放置Callout的同时，生成相应的Drawing Model和Sheet Model。

_CE版在Drawing、Annotate、Detailing栏目内，使用Plan Callout或Section Callout都可以。Section Callout的区别是放置过程中需设置剪切范围高度。_

![](/img/2022/2022-09-04-14-57-25.png)

![](/img/2022/2022-09-04-15-30-58.png)

![](/img/2022/2022-09-04-15-33-35.png)

### 生成平面图模型显示切面线

移到生成的Drawing Model，抽出断面（Cross Section）或者断面线。 关键点：只显示Cut面，关掉Forward和Backward的显示。

生成Drawing是必需的，而Sheet则看是否需生成图纸，可选可不选，Sheet Model是参考Drawing Model。后续的提取断面线需在打开Drawing视图（Plan View）后在参考管理对话框内进行，只有Drawing才可以将剖面的剖切边界线设置为Legacy。

![](/img/2022/2022-09-04-14-57-47.png)

![](/img/2022/2022-09-04-14-58-05.png)

### 提取剖切面断面线

将抽出的断面线转成合法的Line String元素类型。 关键点：把参考过来图形的Visible Edges设置从Dynamic改称Legacy就可以。

在视图窗口左下角打开视图列表，选择相应的Plan view(或Section View)激活相应的Drawing视图窗口，然后打开参考管理，在参考列表中对相应的参考进行设置：

1、选中参考，在属性列表中对Presentation进行设置，关闭前、后剖切方各，只留Cut剖切面；

2、对参考设置Visible Edges为Legacy。

![](/img/2022/2022-09-04-14-59-59.png)

### Q&A 
   1. 为什么在Design Model的View Attributes窗口中看不到Clip Volume Settings选项？
        
        打开Design Model，把鼠标放在已放置的Callout Icon上，会显示一个Popup的工具条，中间有一个工具叫做“Clip Model by Callout”，点击一次此工具之后，就会在View Attributes窗口中看到Clip Volume Settings选项了。
        
   2. 为什么在Drawing Model的View Attributes窗口中看不到Clip Volume Settings选项？ 
        
        打开Drawing Model，并打开Reference窗口，通过菜单Settings> Presentation打开Reference Presentation窗口，其中有Clip Volume Settings设置，关闭Forward和Backward，只留Cut面的显示就可以。 因为Drawing Model是参考了Design Model中的内容，所以它的Clip Volume Settings设置不在View Attributes下，而在各自的Reference Presentation中进行。

   3. 为什么在Sheet Model的Reference窗口中Visible Edges列没有Dynamic，Cached，Legacy的设置？ 
        
        Sheet Model是Design Model的第二层嵌套参考，你需要回到第一层嵌套参考的Drawing Model中进行Reference的Visible Edges设置。

断面线的生成过程请演示视频请参阅：[Bentley 中国优先社区](http://communities.bentley.com/communities/other_communities/chinafirst/m/bentley__-_gallery/270437)

_本地文件：Plan(Section)Callout生成剖面提取断面线(MeshCreateCrossSectionLine).wmv_

