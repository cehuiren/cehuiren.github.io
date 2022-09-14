---
layout: post
title:  "MicroStation绘制路径放置LumenRT车辆"
date: 2022-09-10
categories: MicroStation
tags: MicroStation LumenRT 车辆 路径
excerpt: MicroStation中绘制路径并加载LumenRT中的车辆单元。
author: 测量老覃
---
* content
{:toc}

视频教程：[LumenRT软件操作视频_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1J441177x2?p=16)

### MicroStation使用LumenRT车辆

1. 在MS里绘制路径，样条曲、智能线等，将其设置为构造类型，并移到地面高程；
2. 可视化工作流、动画模拟（Animate）、交通，创建车道、单车道或双车道；
![](/img/2022/2022-09-10-08-57-41.png)
3. 点击1处的…挖钮：
![](/img/2022/2022-09-10-08-57-53.png)
4. 点上图1处新建车辆集名称，在2处命令车辆集名称；
![](/img/2022/2022-09-10-08-58-02.png)
5. 点上图3处加载LRT车辆单元：
![](/img/2022/2022-09-10-08-58-12.png)
6. 在添加车辆对话框内点浏览按钮，找到交通工具单元库目录：
![](/img/2022/2022-09-10-08-58-25.png)
![](/img/2022/2022-09-10-08-58-33.png)
7. 在打开的单元库里选择需要的车辆：
![](/img/2022/2022-09-10-08-58-41.png)
注意如果要使车辆没路径行走，需将车辆旋转角设置为90度。
![](/img/2022/2022-09-10-08-58-51.png)
8. 回到创建车道对话框、设置好参数，选择前面创建的路径即可：
![](/img/2022/2022-09-10-08-58-59.png)
9. 选择路径和方向确定后如下完成车道添加
![](/img/2022/2022-09-10-08-59-06.png)
在放置车辆后需在MicroStation里将车辆所在图层Vehicles关闭，否则会在LumenRT里同路径上存在停止不行走的车辆。添加路径和车辆前需要存在至少一个行驶平面。

### LumenRT单元库路径

    在MicroStation中使用的LumenRT单元库文件已保存备份在：\华为云盘\LumenRT\MicroStation中使用的LRT资源

> The install location for the LumenRT content cell libraries on my PC is 
> C:\ProgramData\Bentley\connectsharedcontent\CellsForLumenRT 
> 
> Cell libraries include:
> 
> Cars.cel
> 
> Construction_Equipment.cel
> 
> LRT_Buildings.cel
> 
> LRT_Characters.cel
> 
> LRT_Misc_Transporation.cel
> 
> LRT_Plants.cel
> 
> LRT_Rocks.cel
> 
> Semi._Trucks.cel
> 
> SUV & PU Trucks.cel
> 
> Vans and Buses.cel

原文：[Traffic Problem - Visualization Forum - Visualization - Bentley Communities](https://communities.bentley.com/products/microstation/microstation_visualization/f/visualization-forum-1977638070/181122/traffic-problem/538538)