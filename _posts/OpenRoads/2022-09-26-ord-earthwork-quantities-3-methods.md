---
title: OpenRoads计算工程量(挖填方量)的三种方法
author: QinDong
date: 2022-09-26 20:56:00 +0800
categories: OpenRoads 体积方量
tags: 挖填方 工程量 方量
excerpt: 
---
* content
{:toc}

![](/img/2022/2022-09-26-20-57-58.png)

### 1、廊道挖填方量报表

对于已存在的廊道，生成廊道方量报表步骤：

ORD-Modeling▶Corridor▶Review▶Corridor Reports，选择廊道，生成部件体积报表，表中包含挖、填方量。

![](/img/2022/2022-09-26-20-58-21.png)

使用Dynamic Sections动态查看横断面，视图属性中打开挖填图形、挖填方量值显示：

![](/img/2022/2022-09-26-20-58-36.png)

### 2、廊道地形模型、激活地形模型挖填体积分析
- 由廊道创建对应地形模型：ORD-Modeling▶Terrain▶Create▶Additional
     Methods▶Create Corridor Alternate Surface，选中廊道，生成廊道对应地形模型；

![](/img/2022/2022-09-26-20-58-49.png)

- 两期地形计算方量：ORD-Modeling▶Terrain▶Analysis▶Volumes▶Analyze Volume，选择Terrain Model To Terrain Model Volume方式，按提示指定两期模型，结果放置在指定位置。

![](/img/2022/2022-09-26-20-59-03.png)

存在两期地形的情况下也可以使用ORD-Modeling▶Analysis▶Volumes▶Create Cut Fill Volumes生成挖、填体积块。注意其中一期地形必须为Exsisting类型。

### 3、平行断面法报表
- 创建挖、填体积块：ORD-Modeling▶Home▶Model Analysis and  Reporting▶Civil Analysis▶Create Cut Fill Volumes（ORD-Modeling▶Analysis▶Volumes▶Create Cut Fill Volumes），在存在两期地形模型时可以生成挖填体积块，在存在廊道和现有地形模型的情况应该也可以直接生成体积块，是否需要先由廊道生成对应地形模型尚待验证。

![](/img/2022/2022-09-26-20-59-14.png)

![](/img/2022/2022-09-26-20-59-20.png)

- 放置命名边界（平行横断面法必须先放置命名边界）：ORD-Modeling▶Drawing Production▶Named Boundaries▶Place Named Boundary放置横断面命名边界（Place Named Boundary Civil Cross Section），按提示指定范围。

![](/img/2022/2022-09-26-20-59-34.png)

- 横断面工程量报表：ORD-Modeling▶Home▶Model Analysis and  Reporting▶Civil Analysis▶End Area Volumes Report生成横断面工程量报表。

![](/img/2022/2022-09-26-20-59-43.png)

上述三种方法计算统计挖、填工程量数值差异不大，可相互对照验证。

[Earthwork Quantities in OpenRoads Designer](https://www.youtube.com/watch?v=I-eAcHdllLc&list=PLnJUnxLwu_N6bmJnO4jSyVmADThsPyx1X&index=8&t=26s&ab_channel=BentleyOpenRoads)