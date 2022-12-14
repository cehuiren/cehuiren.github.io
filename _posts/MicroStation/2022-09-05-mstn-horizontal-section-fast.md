---
layout: post
title:  "MicroStation快速水平剖切"
date: 2022-09-05
categories: MicroStation 剖切
tags: MicroStation 剖面 断面 key-in
excerpt: MicroStation创建水平剖面生成剖面线的方法。
author: 测量老覃
---
* content
{:toc}

### 剖面对话框

使用Dialog Section键入命令打开剖面对话框进行剖切可以快速输出到新的设计文件。

![](/img/2022/2022-09-05-08-27-23.png)

### 水平剖切步骤：

	1. 将模型置为顶视图；
	2. 绘出一个含三个点的直线折线对象（用于三点定位水平面）；
	3. 使用Set Elevation将定位对象设置为剖切高程；
	4. 使用Section By Plane进行剖切，捕捉定位对象上的三个端点，再在其它位置点左键确认完成剖切。

![](/img/2022/2022-09-05-08-27-40.png)

![](/img/2022/2022-09-05-08-27-49.png)

1、将定位平面线对象设置为86高程

![](/img/2022/2022-09-05-08-28-07.png)

2、打开Dialog Section对话框，打开或创建输出文件（新建必须用3D种子）

![](/img/2022/2022-09-05-08-28-23.png)

![](/img/2022/2022-09-05-08-28-41.png)

3、使用平面剖切

![](/img/2022/2022-09-05-08-28-53.png)

4、捕捉平面定位对象上的三个点，再按左键确定完成剖切

![](/img/2022/2022-09-05-08-29-02.png)

### 注意事项

	• 对于复杂的模型，使用Project Element剖切或对象剖切可能都会有问题，此时可从断面布置线上绘一垂直的矩形，使用Element进行剖切成功，其它剖切方式可能都会出现杂乱对象。

![](/img/2022/2022-09-05-08-29-13.png)

![](/img/2022/2022-09-05-08-29-39.png)

![](/img/2022/2022-09-05-08-29-49.png)

	• 对于参考进来的模型，若模型对象是参数化对象，需转换为网格或曲面方可剖切出来。
	• 点击对象剖切命令后选择对象，点击左键两次完成剖切。
