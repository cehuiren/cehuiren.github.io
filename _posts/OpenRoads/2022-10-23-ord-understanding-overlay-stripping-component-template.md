---
title: OpenRoads Designer Understanding Overlay(Stripping) Components
author: QinDong
date: 2022-10-23 10:58:00 +0800
categories: OpenRoads 断面模板
tags: 模板 表土 en 组件
excerpt: 
---
* content
{:toc}

## Overlay Components
- An Overlay/Stripping Component has special properties to follow the designed component slopes or the active surface or both.
- The top or the bottom or both may vary in position.
- The linestring from which the component is defined is referred to as the "spine."
- We define a string of points (or "line" or "open shape")to set the geometry of the component.
- The Top is always the top limit of the component and the Bottom is always the bottom.This "spine" is not necessarily the top of the component,but will always be between the Top and the Bottom of the component.

![](/img/2022/2022-10-24-09-18-52.png)

### Top Options
- Follow Surface: The top of the component will be draped along the top of the target surface.
- Follow Component: The top of the component will follow the component spine.
- Follow highest: The top of the component will follow the higher of the surface or the spine.

### Bottom Options
- Follow Surface: The bottom of the component will follow the target surface at a distance below the surface as defined by the Surface Depth.
- Follow Component: The bottom of the component will follow the component spine at distance below it as defined by the Component Depth.
- Follow Lowest: The bottom of the component will follow the lower of the surface line or the spine minus the Component Depth.
- Follow Highest: The bottom of the component will follow the lower of the surface line or the defined component line at the specified depths.

### Nominal Depth Milling Only
- Example (1" Depth Following Existing Ground)
![](/img/2022/2022-10-24-09-34-52.png)

![](/img/2022/2022-10-24-09-37-15.png)

![](/img/2022/2022-10-24-09-38-34.png)

- Example (2" Depth BELOW PROFILE Correcting Cross Slope)
![](/img/2022/2022-10-24-09-40-43.png)

![](/img/2022/2022-10-24-09-44-03.png)

![](/img/2022/2022-10-24-09-44-57.png)

![](/img/2022/2022-10-24-09-46-00.png)

### Nominal Depth Milling + Crown Correction Leveling Binder
- Requires TWO Components
- Example (1" Depth Following Existing Ground + Leveling Binder to Correct Cross Slope)
- Top of Leveling Component uses _Active_ Profile
![](/img/2022/2022-10-24-09-51-28.png)

### Slope Correction Milling + Slope Correction Leveling Binder
- Requires _Two_ Components
- Example (2" Depth BELOW PROFILE + Leveling Binder to Correct Cross Slope)
- Top of Leveling Component uses _Active_ Profile
![](/img/2022/2022-10-24-09-57-58.png)

![](/img/2022/2022-10-24-10-00-48.png)