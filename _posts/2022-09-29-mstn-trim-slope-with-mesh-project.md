---
title: MicroStation开挖边坡与地形网格裁剪（Mesh交线法）
author: QinDong
date: 2022-09-29 20:33:00 +0800
categories: MicroStation 网格
tags: 裁剪 网格 边坡 地形
excerpt: 
---
* content
{:toc}

地形为Mesh对象，用曲面模型工具创建边坡曲面后转换为网格，使用网格模型工具进行裁剪（求交线工具在曲面模型的应用工具中）。
- 根据坡脚线、坡比绘坡度线；
- （Surface）由Extrude Along创建坡面Surface，必要时用Extend surface将曲面进行拉伸，使得坡面超出地形与地形产生交线；
- （Mesh）由元素创建Mesh，选择刚生成的坡面曲面，将坡面转换为Mesh，对于圆弧段、渐变段需将chord设为一个很小的值；
- （Surface）由Surface Utilities中的Compute Surface计算Mesh的交线，选中地形Mesh、坡面Mesh，确认完成两网格的交线创建；

![](/img/2022/2022-09-29-20-33-23.png)

- （Mesh）使用Mesh Project将坡面Mesh用两Mesh交线进行裁剪，由于难于确定内、外侧，可以选择全部保留（Both），后面人为删除不需要的一侧，方向一般选正交，对于有些特殊情况比如人为绘出的裁剪线不一定在Mesh上，可以将视图设为顶视图，方向选择视图进行裁剪

![](/img/2022/2022-09-29-20-33-37.png)

![](/img/2022/2022-09-29-20-33-44.png)

对于设计图上绘出的2D开挖范围线，要使之投射到地形曲面上，也可以用Mesh Project，方法可以选择投影Project或压印Imprint。