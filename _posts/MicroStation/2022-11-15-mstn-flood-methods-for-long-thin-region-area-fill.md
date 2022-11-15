---
title: MicroStation狭长区域泛填（面积、填充）操作方法
author: QinDong
date: 2022-11-15 11:10:00 +0800
categories: MicroStation 基础
tags: 面积
excerpt: 
---
* content
{:toc}

A previous tip detailed that using any of the Flood Methods in MicroStation requires the entire region being flooded by visible in the view window.

A Commenter asked how to do this for a long, thin region where you couldn’t identify a point inside the area.

Here is how to accomplish that task…

In the image below we have a long roadway segment. There is a 25′ wide strip along the right side of the center line we want to fill.

![](/img/2022/2022-11-15-16-11-42.png)

- Zoom in on the shaded area (or any area along the region) so that we can identify a blank area inside the region to be flooded.

- Select the flooding tool (in this case Create Region) and configure the desired tool settings

- Place a tentative point somewhere inside the area to be flooded for the flood point – The tentative crosshair will have a dashed symbology

![](/img/2022/2022-11-15-16-12-07.png)

- Use your mouse scroll wheel to zoom out so the entire region is visible

![](/img/2022/2022-11-15-16-12-16.png)

- Click a data point (<**D**>) to select the tentative point
- The bounding region will highlight
- <**D**> again to complete the flood
- The region is now filled.

![](/img/2022/2022-11-15-16-12-25.png)