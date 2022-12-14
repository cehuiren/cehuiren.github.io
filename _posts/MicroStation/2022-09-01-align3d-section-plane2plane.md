---
layout: post
title:  "三维对齐将剖面结构图（断面）快速放置至剖切面"
date: 2022-09-01
categories: MicroStation 剖切
tags: 断面 剖面 MicroStation
excerpt: 在进行三维建时，需将剖面结构图（横断面图）放置到3D轴线端点剖切面上（铅垂、或垂直于3D轴线平面），一般通过跨视图拷贝、粘贴的方式完成，即在顶视图上拷断面剖结构图对象，再到剖切平面进行粘贴，现通过三维对齐能更快速的进行复制定位。
author: 测量老覃
---
* content
{:toc}

在进行三维建模时，需将剖面结构图（横断面图）放置到3D轴线端点剖切面上（铅垂、或垂直于3D轴线平面），一般通过跨视图拷贝、粘贴的方式完成，即在顶视图上拷断面剖结构图对象，再到剖切平面进行粘贴。通过跨视图拷贝、粘贴需在两个视图窗口内分别置为顶视图、剖切面视图，在顶视图窗口内拷贝对象，到剖切面视图窗口中粘贴即可。将窗口置为剖切面视图另文说明。在MS里有更方便快捷的操作方法，那就是三维对齐工具，以下为通过三维对齐快速进行复制定位的操作步骤。

### MicroStation三维对齐操作方法

● 将剖面结构图生成一个线串、形状等单一图形对象，如果是多个对象，需分别操作，三维对齐每次只对操作一个对象；

● 使用三维对齐命令，方法为平面对平面（Plane to Plane）;

● 捕捉移动对象的定位基点（通常为与轴线的交点）作为平面原点；

● 点击移动对象的一个特征点作为定义平面X轴的方向；

● 点选第三个点定义平面的Y轴方向；

● 三点中第一个点即原点定位基点、X轴的方向点必须与目标位置一致，基点必须重合，X轴方向点一致，但不需重合；

● Y轴方向点的移动对象与目标定位对象可不一致，只需分别在各自平面Y轴方向的任意一点即可；

● 移到剖切面上，点选基点，目标平面X轴的正向点、Y轴正方向上任意一点，确认完成：

### 剖面断面图选中及定位:

1、捕捉定位基点并选中移动对象

![](/img/2022/2022-09-02-16-35-20.png)

2、捕捉对象平面X轴正向点

![](/img/2022/2022-09-02-16-35-38.png)

3、捕捉对象平面Y轴正向上任意一点

![](/img/2022/2022-09-02-16-35-49.png)

4、移动对象三点后效果

![](/img/2022/2022-09-02-16-35-59.png)

5、剖切面上的定位基点即平面原点

![](/img/2022/2022-09-02-16-36-09.png)

6、与移动对象平面X轴对应的正点定位点

![](/img/2022/2022-09-02-16-36-19.png)

7、目标平面Y轴正向上任意一点,可与移动对象平面第三点位置不重合

![](/img/2022/2022-09-02-16-36-28.png)

8、确认完成移动对齐

![](/img/2022/2022-09-02-16-36-37.png)
