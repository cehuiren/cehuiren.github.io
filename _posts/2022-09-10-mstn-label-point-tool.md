---
layout: post
title:  "MicroStation坐标标注（Label Coordinate）工具简介"
date: 2022-09-10
categories: MicroStation
tags: MicroStation 坐标 AccuDraw key-in 标注
excerpt: MicroStation坐标标注工具及只标注高程的方法。
author: 测量老覃
---
* content
{:toc}

【工具简介】
1. 对元素上的某个点进行对应坐标标注时，可以使用**Label Coordinate**工具。调用位置是MicroStation主菜单的**Tools>Dimensions>XYZ Text>Label Coordinate**, 然后选择需要标注的点即可。
2. 也可以直接输入Key-in命令：**Label Point** 来调用此工具
3. 如果只想标注Z坐标值，则先输入Key-in命令：**label hidexy on**，然后再使用该工具，即可如下图所示只标注Z坐标的值。
![](/img/2022/2022-09-10-14-05-33.png)