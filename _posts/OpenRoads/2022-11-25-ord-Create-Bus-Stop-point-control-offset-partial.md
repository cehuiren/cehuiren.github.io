---
title: OpenRoads Designer道路旁添加公交车停车区
author: QinDong
date: 2022-11-25 08:37:00 +0800
categories: OpenRoads 道路廊道
tags: 点控制
excerpt: 
---
* content
{:toc}

![](/img/2022/2022-11-25-08-42-03.png)

在道路旁添加如上图所示的公交车停车区操作步骤：

![](/img/2022/2022-11-25-08-50-19.png)

1、使用Geometry下的Single Offset Partial将路线中的一段进行偏移：

![](/img/2022/2022-11-25-08-51-08.png)

参数设置偏移距离、起点、偏移长度：

![](/img/2022/2022-11-25-08-54-32.png)

2、使用Geometry下的Variable Offset Taper创建停车区两端的过渡区：

![](/img/2022/2022-11-25-08-56-14.png)

![](/img/2022/2022-11-25-08-57-05.png)

3、使用Geometry中的Arc添加简单圆弧，在折角处添加圆弧：

![](/img/2022/2022-11-25-09-00-50.png)

![](/img/2022/2022-11-25-08-59-48.png)

4、将偏移对象、圆弧生成一个整体复合对象：

![](/img/2022/2022-11-25-09-01-47.png)

创建复合对象时打开特征定义工具栏：

![](/img/2022/2022-11-25-09-02-44.png)

在特征定义工具栏内将生成复合对象的特征定义设为：Road_EdgeOfPavement

![](/img/2022/2022-11-25-09-04-53.png)

5、在廊道、廊道编辑菜单下选择添加点控制，停车区路边缘线对应的点添加为停车区边线约束：

![](/img/2022/2022-11-25-09-08-35.png)

添加点控制时选中路线中线、捕捉停车区边线两端端点、提示定位点时选中路线边线、Locate Plan of Profile Element选中以上生成的停车区边线即可。

![](/img/2022/2022-11-25-09-15-14.png)