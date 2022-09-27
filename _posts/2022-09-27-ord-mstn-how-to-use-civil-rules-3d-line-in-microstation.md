---
title: MicroStation里如何使用OpenRoads带有土木规则的3D平面线
author: 网格转载
date: 2022-09-27 15:29:00 +0800
categories: OpenRoads 土木规则
tags: 平面几何 立面几何 规则 OpenRoads MicroStation
excerpt: 
---
* content
{:toc}

## 问题描述
带有土木规则**Civil Rules**的3D平面线如何在**MicroStation**软件里应用？因为带有土木规则的平面线，Microstation的一些基本工具是用不了的，比如移动等，现在介绍两种方法，解决这个问题。

## 解决方案

## 第一种方法
- 第一步，新建一个3D文件，文件名为TEST；
- 第二步，在TEST文件里把Powercivil设计完成的3D平面线参考进来；
- 第三步，在参考的对话框里，点击“合并到主文件里Merge into Master”；

![](/img/2022/2022-09-27-14-46-25.png)

- 第四步，第三步操作完成后，带有3D的平面线即可在Microstation进行操作。

### 第二种方法
- 第一步，在3D的平面线文件里，放置围栅Place Fence，把路线全部放在围栅里；

![](/img/2022/2022-09-27-14-46-35.png)

- 第二步，在围栅的工具，找到”Copy/Move Fence Contents to File,并打开这个工具；

![](/img/2022/2022-09-27-14-46-44.png)

- 第三步，在Output File里选择一个文件为保存拷贝Fence里内容；

![](/img/2022/2022-09-27-14-46-50.png)

- 第四步，第三步操作完成后，带有3D的平面线即可在Microstation进行操作。