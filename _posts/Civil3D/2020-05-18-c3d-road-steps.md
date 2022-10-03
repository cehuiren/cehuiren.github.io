---
layout: post
title:  "AutoCAD Civil 3D公路工程建模步骤"
date: 2020-05-18
categories: Civil3D 道路
tags: Civil3D 道路 廊道 建模
excerpt: 这是以前在一个县级公路施工时建模过程的笔记，简短介绍了创建公路（廊道）模型的步骤。
author: 测量老覃
---
* content
{:toc}

## 曲面、路线

### 原始曲面

处理原始地形如等高线、地形点等，检查高程是否正确，生成原始曲面。

### 转折点线路图

收集、检查路线折点坐标并绘制转折点线路图；

### 主路线

收集平曲线参数，绘制主路线图。如分段则需注意各分段起始桩号不能直接使用折点桩号，最好使用其中一个百米桩开始；

### 曲面纵断面

收集竖曲线参数，绘制曲面纵断面和纵断面图。在纵断面图特性、标注栏内增删纵断面图上显示内容如地面高程、挖填高度等；

## 设计纵断面

### 布局纵断面

![](/img/2020/2022-09-06-13-48-14.png)

![](/img/2020/2022-09-06-13-48-25.png)

- 根据竖曲线折点创建竖曲线折点文本文件，根据此文件绘制布局纵断面折点图，在折点图上根据竖曲线半径在各折点上补绘竖曲线；在设计图中找到以上纵坡折点视图，按折点桩号、高程（空格分隔）格式生成如下文本文件：111520 2893.72。再根据文件绘制纵断面即可完成布局纵断面的折点绘图，再在折点上按竖曲线半径添加曲线即可。注意：文本文件中的桩号不能超出纵断面图的桩号，否则前一段不会绘出。出现超出部分则应按桩号、坡比计算出纵断面图起点的桩号与设计高程作为文件内容的第一行内容。
- 
![](/img/2020/2022-09-06-13-48-42.png)

- 根据文件绘出的布局纵断面如上图中蓝线。再按竖曲线参数补绘曲线。选中断面线，在右键菜单中点击“编辑纵断面形状”打开纵断面图编辑工具栏：
- 
![](/img/2020/2022-09-06-13-48-51.png)

![](/img/2020/2022-09-06-13-48-59.png)

- 在第一组工具列表中的曲线设定里设置曲线参数：
- 
![](/img/2020/2022-09-06-13-49-09.png)

![](/img/2020/2022-09-06-13-49-20.png)

- 再用第一组命令列表中的绘制曲线切线，依次点击曲线两切线的三个端点完成曲线绘图：
- 
![](/img/2020/2022-09-06-13-49-36.png)

![](/img/2020/2022-09-06-13-49-48.png)

- 绘竖曲线时依次点图中三点，图中红色部分即为竖曲线。后续曲线类似，如第3点的曲线从第2点开始依次点3及后面的折点。绘曲线前推荐在“编辑纵断面样式”里将变坡点标记选择打开如方框，便于捕捉端点。

## 偏移路线、加宽

### 偏移路线及加宽

- 根据路面宽度创建两侧偏移路线，并在偏移路线上添加加宽区域。有关加宽见相关博文。

![](/img/2020/2022-09-06-13-49-57.png)

- 为了加宽准确，建议采用自动加宽中的手工模式逐个曲线添加加宽。自动加宽需先给主路线添加速度与设计标准如下三张图，但自动加宽的手工模式（手动指定加宽段）可忽略选择标准文件直接执行“继续当前命令”：

## 超高、装配及采样

### 超高

- 通过修改编辑设计规范自动完成超高计算，也可按设计图中的超高视图创建超高数据表后导入。关于自动超高与手动超高数据的导入参见相关博客。

- 注意：通过设计规范自动完成的超高数据表在绘超高视图时会出现混乱。可能是因自动完成的超高计算数据项有多余的部分。手动编辑创建超高数据只需6个关键点的左、右横坡数据，绘出的超高视图与设计一致。
装配

- 根据磺断面尺寸完成装配，也可分段创建多个装配。在创建道路时指定桩号范围匹配特定装配。创建道路时指定桩号、步长、和逻辑目标等。
采样

- 创建道路后可直接采样并绘横断面图。道路曲面在生成时会出现局部不按要求连接的情况，而且在道路上直接采样，横断面与装配一致，而在道路曲面上采样，则会出现部分折点不连接，需要手动修改断面图。