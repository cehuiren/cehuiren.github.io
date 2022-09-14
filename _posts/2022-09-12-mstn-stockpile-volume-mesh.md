---
layout: post
title:  "MicroStation料堆网格法方量计算"
date: 2022-09-12
categories: MicroStation
tags: MicroStation 方量
excerpt: MicroStation中直接将料场地形生成网格计算料堆体积方量。
author: 测量老覃
---
* content
{:toc}

1、将地形点读入：Drawing>Annotate>Terrain Model>Import Coordinates

![](/img/2022/2022-09-12-08-46-27.png)

2、Modeling>Mesh>Create>Mesh from Points生成网格

![](/img/2022/2022-09-12-08-46-34.png)

3、适当删除边界上多余的小平面，并调整部分小平面边与实际相符

4、Modeling>Mesh>Mesh Utilities>Extract Boundary提取网格边界（如前图中红色部分）

5、提取的边界是线串，需将其转换成Complex Shape后才能使用Mesh From Element转换成网格

![](/img/2022/2022-09-12-08-46-42.png)

![](/img/2022/2022-09-12-08-46-53.png)

![](/img/2022/2022-09-12-08-46-59.png)

![](/img/2022/2022-09-12-08-47-19.png)


6、使用Modify Meshes>Stitch into Mesh将两个Mesh缝合成一个封闭的网格

![](/img/2022/2022-09-12-08-47-28.png)

7、选中网格，查询属性即可得到体积数据

![](/img/2022/2022-09-12-08-47-42.png)

