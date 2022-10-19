---
title: OpenRoads Designer平面几何路线、纵断面几何基础绘图
author: QinDong
date: 2022-10-19 09:39:00 +0800
categories: MicroStation 纵断面几何
tags: 纵断面 纵剖面 几何 平面几何
excerpt: 
---
* content
{:toc}

## 平面几何
绘平面几何路线前先打开土木精确绘图并激活：

![](/img/2022/2022-10-19-09-29-34.png)

### 直线侧
需按坐标输入转折点时激活土木精确绘图中的直角坐标图标。

![](/img/2022/2022-10-19-09-31-53.png)

![](/img/2022/2022-10-19-09-32-17.png)

### 添加缓-圆-缓曲线
在转折点处两直线间添加缓和曲线和圆曲线：Horizontal\Arcs\Arc Between Elements\Spirl Arc Spirl：

![](/img/2022/2022-10-19-09-33-34.png)

输入参数：

![](/img/2022/2022-10-19-09-36-15.png)

选择直线段切线：

![](/img/2022/2022-10-19-09-36-02.png)

## 纵断面几何
纵断面绘图前先打开土木精确绘图：

![](/img/2022/2022-10-19-09-06-15.png)

### 直坡段

按Tab键在各参数选项间切换，按左右箭头选择参数输入方式：

![](/img/2022/2022-10-19-09-09-03.png)

### 竖曲线

两纵断面直线间添加圆曲线竖曲线：Vertical\Curves\Profile Curves Between Elements\Circular Curve Between Elements：

![](/img/2022/2022-10-19-09-11-03.png)

按竖曲线半径或长度指定参数绘制。在两元素间插入曲线前两项为抛物线和不对称抛物线，第四项也可更灵活的选择偏移、圆曲线和抛物线：

![](/img/2022/2022-10-19-09-27-23.png)

纵断面复合：Vertical\Complex Geometry\Profile Complex By Elements：

![](/img/2022/2022-10-19-09-13-15.png)