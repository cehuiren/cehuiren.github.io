---
title: OpenRoads Designer加宽表
author: 中国优先社区
date: 2022-09-20 17:13:00 +0800
categories: OpenRoads 道路廊道
tags: OpenRoads 加宽 模板 参数
excerpt: 
---
* content
{:toc}

加宽表文件（.wid）：

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

- **rad**：平曲线半径
- **Wi**：内侧加宽值
- **Li**：内加宽过渡段长
- **Wo**：外侧加宽值
- **Lo**：外加宽过渡段

举个例子，R=20m，加宽4m，则设置为：rad=20，Wi=4，Wo=0，Li和Lo可为任意值不影响。
表格里的第一行rad=0,Wi=3.5指的是默认道路宽度为3.5m，这个不是加宽宽度。
rad半径值为向下取值。超过50m不加宽可设置为rad=50，Wi=1.5。

若平曲线不含缓和曲线，需在表格中设置L加宽渐变过渡值。