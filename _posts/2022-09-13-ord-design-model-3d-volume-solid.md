---
layout: post
title:  "OpenRoads Designer设计模型与现有地形创建三维挖填方体计算方量"
date: 2022-09-13
categories: OpenRoads 体积方量
tags: MicroStation OpenRoads 方量 挖填方 
excerpt: OpenRoads中使用MicroStation创建的设计模型网格赋于特征与地形模型创建挖填三维方体进行挖填方工程量的计算。
author: 测量老覃
---
* content
{:toc}

OpenRoads中使用MicroStation创建的设计模型网格赋于特征与地形模型创建挖填三维方体进行挖填方工程量的计算。

<div style="text-align:center;"><img src="/img/gif/ord-design-model-volume.gif"></div>

>3D Volumes & Earthwork Overview
>
>Earthwork quantities are now calculated from 3D mesh elements. You no longer need to create a set of cross sections to get cut and fill volumes. 
>Earthwork quantities are now truly 3D. 
>The Create Cut Fill Volumes tool creates a 3D mesh element for cut and a 3D mesh element for fill. 
>These meshes can then be used to extract earthwork quantities and reports directly from the 3D Model.
>3D cut/fill meshes can be created from terrains, corridors, linear templates, civil cells or any 3D mesh element that has a civil feature definition.
>The software scans the DGN file for Feature Definitions that represent Existing and Design elements. 
>For Design elements a bottom mesh is automatically formulated (in memory) and is used to compare to the existing elements (typically an existing ground terrain). 
>The result of this process is 3D cutfill mesh elements that represent cut and fill volumes.


对于设计模型，在转换成网格并缝合成一个整体后，还需添加土木特征方可参与计算方量：
1. 新建一ORD文件，种子为2D；
2. 创建原始地形（或其它地形），类型为Exsisting类型；
3. 创建地形后会自动添加3D模型，转到3D模型内；
4. 将设计模型网格参考、合并进3D模型；
5. 添加土木特征：ORD-Modeling、Geometry、General Tools、Standards、Set Feature Definition，选中设计模型网格，类型选Mesh，特征可选列表中任意一种即可；
6. 使用Create Cut Fill Volumes工具，创建3D挖填方量，按提示完成即可；
7. 创建3D体积块后，除了可以查询体积块的体积属性外，还可以使用命名边界工程量报表工具生成报表，在提示选择命名边界时，选无确定即可。

<div style="text-align:center;"><img src="/img/2022/2022-09-13-10-17-03.png"></div>

<div style="text-align:center;"><img src="/img/2022/2022-09-13-10-17-15.png"></div>
<div style="text-align:center;"><img src="/img/2022/2022-09-13-10-17-29.png"></div>
<div style="text-align:center;"><img src="/img/2022/2022-09-13-10-17-38.png"></div>

>视频名称：Module 3 - 3D Volumes & Earthwork