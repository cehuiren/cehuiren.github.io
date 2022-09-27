---
title: OpenRoads公路(廊道)组件工程量及挖填方量统计计算
author: QinDong
date: 2022-09-27 09:01:00 +0800
categories: OpenRoads 体积方量
tags: 公路 廊道 工程量 组件 方量 挖填方
excerpt: 
---
* content
{:toc}

- 廊道及组件方量统计：Corridors\Review\Corridor Reports，然后选择廊道（图一）；

![](/img/2022/2022-09-27-08-55-10.png)
_图一_

- 动态显示横断面：Dynamic Sections

- 两期地形方量计算：如果计算廊道需先使用Terrain\Additional Methods\Create Corridor Alternate Surfaces创建廊道模型地形（图二），然后使用Terrain\Analyze Volume计算两期地形方量（图三）

![](/img/2022/2022-09-27-08-55-18.png)
_图二_

![](/img/2022/2022-09-27-08-55-26.png)
_图三_

- 创建挖填方量体积模型：Home\Model Analysis and Reporting\Civil Analysis\Create Cut and Fill Volume（图四）；

![](/img/2022/2022-09-27-08-55-34.png)
_图四_

- 断面法土方计算：
 - 放置命名边界（图五）

![](/img/2022/2022-09-27-08-55-42.png)
_图五_

 - 选择路线中心，输入起、止点（图六）

![](/img/2022/2022-09-27-08-55-49.png)
_图六_

 - 生成的命名边界如（图七）

![](/img/2022/2022-09-27-08-55-56.png)
_图七_

 - 返回Home，执行Civil Analysis\End Area Volumes Report（图八）

![](/img/2022/2022-09-27-08-56-03.png)
_图八_

 - 生成的方量报表如（图九）

![](/img/2022/2022-09-27-08-56-10.png)
_图九_

>41 - OpenRoads Designer  Earthwork Quantities in OpenRoads Designer_1080p.mp4
{: .prompt-info}