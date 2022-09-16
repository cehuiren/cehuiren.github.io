---
layout: post
title:  "ORD剪切地形模型分区域两期计算挖填方量"
date: 2022-09-13
categories: OpenRoads 地形模型
tags: OpenRoads 地形模型 剪切 方量 
excerpt: OpenRoads对大形地形模型进行剪切分区并分别计算挖填方量三维体积块的操作过程。
author: 测量老覃
---
* content
{:toc}

利用剪切地形对开挖区域进行分区然后计算各区域挖填方量极其方便，步骤如下：

### 一、分区的创建
1. 将各分区多边形建成一个设计文件；
2. 如果分区多边形不封闭，可以使用Region泛填方式生成Shape，shape可以作为剪切地形时的边界对象使用；
3. 将区域设计文件参考，并合并进地形文件；

### 二、地形
1. 生成用于计算方量的两期地形；
2. 一般对位于上层的地形进行裁剪分区；

### 三、裁剪分区
- 使用地形模型、创建、Additional Methods内的创建剪切地模型；
<div style="text-align:center;"><img src="/img/2022/2022-09-13-15-11-32.png"></div>

- 选择被剪切分区的地形模型、设置剪切方式为外部、指定新生成的地形模型的特征属性和名称，选择需要分区的区域对象（在区域内部点击选取即可）；

<div style="text-align:center;"><img src="/img/2022/2022-09-13-15-11-42.png"></div>

- 按提示完成操作即可；
- 新生成的剪切地形为被剪切地形的一个副本，并添加了边界属性；
- 若需修改新生成的分区地形的属性，直接在浏览器内选中分区地形的名称，修改属性即可，勿选择下部的原地形副本名称。

<div style="text-align:center;"><img src="/img/2022/2022-09-13-15-11-49.png"></div>

四、方量计算

- 地形模块、Analysis区内的Volumes下的Analyze Vulume
<div style="text-align:center;"><img src="/img/2022/2022-09-13-15-11-57.png"></div>

- 按地形对地形方式计算选择区域内的方量即可：

<div style="text-align:center;"><img src="/img/2022/2022-09-13-15-12-14.png"></div>
<div style="text-align:center;"><img src="/img/2022/2022-09-13-15-12-22.png"></div>