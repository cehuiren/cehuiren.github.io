---
layout: post
title:  "MicroStation三维模型输出至GoogleEarth谷歌地球"
author: 测量老覃
date: 2022-09-19
categories: MicroStation 基础
tags: MicroStation 三维模型 Google 坐标系
excerpt: 将MicroStation三维模型设置正确的地理坐标系后导出为交换格式后导入到GoogleEarth实现三维卫星地图上展示建筑模型的方法。
image:
  path: /img/2022/2022-09-19-08-52-12.png
  width: 800
  height: 409
  alt: Google卫星地图上的三维大坝模型
---
* content
{:toc}

将MicroStation三维模型设置正确的地理坐标系后导出为交换格式后导入到GoogleEarth实现三维卫星地图上展示建筑模型的方法。

- 将三维模型文件备份为新文件并整理，如将参考合并，清除多余的线、文字等无用对象；
- 进入顶视图，将所有对象向右平移（宁蓄41000000），为东坐标添加投影带号；
- Drawing工作流、Utilities、Geographic、Coordinates System定义投影参数（投影、西安80、3度、41带）；
<div style="text-align:center;"><img src="/img/2022/2022-09-19-08-46-54.png"></div>
- Drawing工作流、Utilities、Geographic、Google Earth Setting作如下设置：格式为KML and Collada、Altitude为Absolute
<div style="text-align:center;"><img src="/img/2022/2022-09-19-08-47-05.png"></div>
<div style="text-align:center;"><img src="/img/2022/2022-09-19-08-47-12.png"></div>
- Drawing工作流、Utilities、Geographic、Export Google Earth File导出KMZ文件；
<div style="text-align:center;"><img src="/img/2022/2022-09-19-08-47-19.png"></div>
- 打开Google地球（桌面、网页版）添加新项目打开输出的KMZ文件
- 如果位置有偏，在MS里进行适当调整重新导出KMZ再导入GoogleEarth，反复操作直至目视位置合理为止，如果能在卫星地图上找到图上相同的参考点，可根据参考点进行纠偏。