---
title: OpenRoads Designer隧道横断面模板
author: 中国优先社区
date: 2022-10-28 08:28:00 +0800
categories: OpenRoads 断面模板
tags: 隧道 隧洞 横断面
excerpt: 
---
* content
{:toc}

**qiang liMon, Jul 22 2019 9:15 AM**

我用的是七版的ORD，横断面模板中在隧道的Void  Type 中有tunnel,但是自己重新绘制Void Type的时候却不含tunnel,请问这个Void Type中的tunnel是隧道特有的吗？

![](/img/2022/2022-10-28-08-32-10.png)

**Anna An Mon, Jul 22 2019 10:08 AM**

这是ORD新的隧洞模板创建方式，先创建封闭的外圈，然后创建内圈，指定内圈为void type，然后会出现您截图中的non,mesh,tunnel选项，这样创建的是个环形截面。

当然，对于环形模板都可以采用这种方式创建，所以并不是隧道所特有的。

需要注意的是void type为non和mesh时，廊道剪切是传统的二维剪切；void type为tunnel时，廊道剪切是三维剪切，并且会有三个选项。

![](/img/2022/2022-10-28-08-32-24.png)

正确的方法是选中内圈，选择外圈为父组件，然后void type 选项就出来了。

参见 [原文](https://communities.bentley.com/communities/other_communities/chinafirst/f/openroads-powercivil-bridgemaster/182827/ord)