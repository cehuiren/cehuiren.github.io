---
title: MicroStation求两个曲面间的交线
author: 中国优先社区
date: 2022-09-20 08:01:00 +0800
categories: MicroStation 曲面
tags: 曲面 交线 MicroStation
excerpt: 
---
* content
{:toc}

【软件名称】：MicroStation

【软件版本】：CONNECT Edition 10.16.02.34

【用户需求】：请大家先下载如下DGN文件查看其中的图形。它含有两个网格面，目前我们想要获得这两个面的交线。

[SurfaceIntersection.dgn](https://communities.bentley.com/cfs-file/__key/communityserver-wikis-components-files/00-00-00-04-10/SurfaceIntersection.dgn)

【解决思路】：首先想到的第一个工具可能是Mesh Intersect

![](/img/2022/2022-09-20-08-06-01.png)

但测试后发现该工具仅能将最终的曲线嵌入到平面上成为一体。达不到我们的目的。

其次想到的工具可能会是Surface下的Compute Surface Intersections

![](/img/2022/%20.png)

该工具确实能求得一般两个面的交线，但对于该图形操作失败。后来发现是这两个面比较大，超过了500m的SWA（实体工作范围）的限制。用缩放工具将这两个面缩小100倍后就可以得到交线了。然后再将交线放大100倍，移动会原来的位置。这是一个变通的手段，最终是能达到目的但操作太繁琐。

最后发现工具Planar Slice

![](/img/2022/2022-09-20-08-09-07.png)

最合适。启动该工具后直接选那个平面，就能得到交线了。更神奇的是，该工具的名称虽然叫做Planar Slice（平面切分），但它还支持非平面的切分，这一点提请大家注意。所以说，MS中的三维工具有太多的Trick，需要我们不断摸索总结。