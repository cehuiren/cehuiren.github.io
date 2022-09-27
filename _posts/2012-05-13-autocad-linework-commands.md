---
title: AutoCAD编辑处理线构件LINEWORK相关命令
author: QinDong
date: 2012-05-13 08:25:00 +0800
categories: AutoCAD 绘图基础
tags: 编辑 命令 裁剪 延伸
excerpt: 
---
* content
{:toc}

### 1、修剪线条构件
命令：`LINEWORKTRIM `

This command lets you easily trim hatches, lines, polyline, arcs, circles, AEC polygons, mass element extrusions, spaces, or any block-based content (including detail components) made up of these types of linework or objects.
The following prompts are displayed.
```
Select linework to trim
Specify linework, object or block to trim.
Select the first point of the trim line, or, enter to pick on screen.
Specify first point to establish the trim line.
Select the second point to trim line
Specify second point to establish the trim line.
Select the side to trim
Click the side of the trim line where the region you want to remove is located.
```

### 2、选定对象添加多义线边界
命令：`LINEWORKSHRINKWRAP`

You can add a boundary around a selected number of parcels. The boundary is represented by a 2D polyline.

To create a boundary around a selected number of parcels

1. Enter LineworkShrinkwrap at the command line.
2. Select the parcels that you want to use as a boundary and press Enter.This command draws a polyline around touching objects. If your selection includes parcels that are not touching, the boundary is drawn around each individual parcel.

### 3、将线条构件减切
命令：`LINEWORKSUBTRACT`

### 4、将线条构件指定轴上两点居中
命令：`LINEWORKCENTER`

### 5、将线条构件切分
命令：`LINEWORKDIVIDE`

### 6、将线条构件延伸
命令：`LINEWORKEXTEND`