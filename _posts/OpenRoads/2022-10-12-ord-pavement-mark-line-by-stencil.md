---
title: OpenRoads Designer公路标线
author: QinDong
date: 2022-10-12 16:48:00 +0800
categories: OpenRoads 综合基础
tags: 线型 标识 标线
excerpt: 
---
* content
{:toc}

### 创建2D平面标线文件
1、新建标线文件（2D种子）；

2、参考平面路线设计文件；

![](/img/2022/2022-10-12-16-52-42.png)

3、由平面路线平移得到路面标线：

![](/img/2022/2022-10-12-16-54-33.png)

对应特征设置：

![](/img/2022/2022-10-12-16-57-26.png)

### 将2D平面标线压印到三维路面
1、打开三维廊道（公路）模型；

2、参考2D平面标线；

3、进入顶视图；

4、使用Stencil将2D标线印到三维模型路面：

![](/img/2022/2022-10-12-17-06-22.png)

![](/img/2022/2022-10-12-17-08-21.png)

另一种添加标线的方法是在断面模板中添加标线点。在断面模板路面上需要进行标线的位置添加一个标线点（为避免标线重合，可以略高于路面几毫米），特征设置为路面标线（同上）。