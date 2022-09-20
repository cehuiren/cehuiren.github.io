---
title: MicroStation将两个SmartSurface的交集构成实体
author: 中国优先社区
date: 2022-09-20 08:17:00 +0800
categories: MicroStation 曲面
tags: 曲面 实体 MicroStation
excerpt: 
---
* content
{:toc}

1、对两个SmartSurface执行Trim Surfaces操作

![](/img/2022/2022-09-20-08-18-35.png)

选项如下设置：

![](/img/2022/2022-09-20-08-18-51.png)

操作的结果是，两个SmartSurface相互剪切，保留了相交的部分。但此时仍然是SmartSurface。注意Flip 1st和Flip 2nd一定要勾选！

2、执行Stitch Surfaces工具

![](/img/2022/2022-09-20-08-19-02.png)

则两个面构成了一个体。