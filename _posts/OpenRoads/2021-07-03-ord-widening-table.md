---
title: OpenRoads Designer加宽表
author: 中国优先社区
date: 2021-07-03 17:13:00 +0800
categories: OpenRoads 道路廊道
tags: OpenRoads 加宽 模板 参数
excerpt: 
---
* content
{:toc}

曲线加宽用于自动创建和应用水平控件以拓宽曲线附近的车道和/或铺装线边缘，使其在控制路线的每条曲线进一步远离中心线。曲线加宽工具与包含加宽参数的 ASCII 文件 (*.wid) 结合使用。

### 加宽表文件格式

加宽表是一种 ASCII 格式，用户在使用曲线加宽工具时将指向该格式。虽然可能有多个加宽表，但是每次处理只能使用一个表。如果廊道的设计速度发生变化，需要不同的表，则必须多次使用该工具。

### 加宽表文件示例（.wid）：

```
; Cross section type 1, 3 & 4
; Lane width = 3.5 m
;  Rad          Wi              Li              Wo              Lo
0               3.5             2.5             3.5             2.5
20              4.0             25              0               0
25              3.5             25              0               0
30              3.0             25              0               0
35              2.5             25              0               0
40              2.0             25              0               0
45              2.0             25              0               0
50              1.5             25              0               0
```

- **rad**：曲线半径
- **Wi**：曲线内侧加宽
- **Li**：曲线内侧加宽过渡段长
- **Wo**：曲线外侧加宽
- **Lo**：曲线外侧加宽过渡段长

举个例子，R=20m，加宽4m，则设置为：rad=20，Wi=4，Wo=0，Li和Lo可为任意值不影响。
表格里的第一行rad=0,Wi=3.5指的是默认道路宽度为3.5m，这个不是加宽宽度。
rad半径值为向下取值。超过50m不加宽可设置为rad=50，Wi=1.5。

若平曲线不含缓和曲线，需在表格中设置L加宽渐变过渡值。

[操作视频](https://tv.sohu.com/s/sohuplayer/iplay.html?bid=318097949&vars=%5B%5B%22showRecommend%22%2C0%5D%5D&disablePlaylist=true&mute=1&autoplay=false)