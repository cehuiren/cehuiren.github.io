---
layout: post
title:  "MicroStation料堆实体法方量计算"
date: 2022-09-12
categories: MicroStation 实体
tags: MicroStation 方量
excerpt: MicroStation中直接将料场地形生成实体计算料堆体积方量。
author: 测量老覃
---
* content
{:toc}

- C3D内展点、生成曲面，调整曲面
- Output工具导出LandXML
- MS导入交换格式LandXML
  
![](/img/2022/2022-09-12-08-52-39.png)

- Drop成网格（以上步骤可参考网格法料堆方量计算中由地形点创建网格的过程）

![](/img/2022/2022-09-12-08-53-00.png)

- Mesh>Mesh Utilities>Extract Boundary提取边界

![](/img/2022/2022-09-12-08-53-10.png)

- SurFace>Surface Utilities>Conver To Surface
 
![](/img/2022/2022-09-12-08-53-21.png)
 
![](/img/2022/2022-09-12-08-53-32.png)

- Home>Groups>Create Complex Shape将前面提取的边界生成Shape

- Surface>Modify Surfaces>Stitch Surface将网格曲面与底张缝合成一个曲面

![](/img/2022/2022-09-12-08-54-47.png)

![](/img/2022/2022-09-12-08-54-55.png)
 
- 将封闭曲面转换成实体，查询此实体体积即可：
 
![](/img/2022/2022-09-12-08-55-02.png)