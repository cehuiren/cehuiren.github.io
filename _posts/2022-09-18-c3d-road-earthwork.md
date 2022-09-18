---
layout: post
title:  "AutoCAD Civil 3D道路的挖填方量计算及条件部件"
date: 2022-09-18
categories: Civil3D 廊道
tags: 道路 方量 部件 装配 挖填方
excerpt: 上一节中讲了道路创建的三要素：平面路线（Alignment）、纵断面（Profile）、横断面（Assembly），今天就讲一下条件部件在道路模型中的表现以及道路的挖填方计算。
author: 网络转换
---
* content
{:toc}

上一节中讲了道路创建的三要素：平面路线（Alignment）、纵断面（Profile）、横断面（Assembly），今天就讲一下条件部件在道路模型中的表现以及道路的挖填方计算。

### 条件部件

首先创建一个简单的条件部件如下图，实现的功能就是：

填方时：当填方高度小于5m时，直接一坡到地面；当填方高度大于5m时，会出现一级平台，然后再一坡到地面；

挖方时：当挖方高度小于5m时，直接一坡到地面；当挖方高度大于5m时，会出现一级平台，然后再一坡到地面。  

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-52-31.png"></div>

根据上节的步骤进行创建道路模型，见下图所示，仔细观察条件部件在道路模型中的表现。

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-52-41.png"></div>

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-52-50.png"></div>

### 道路的填挖方计算

假定道路模型已经创建完成，本例中使用的横断面见下图： 

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-52-58.png"></div>

若要进行道路的填挖方计算，首先要创建道路曲面，然后再创建采样线（sampleLine），civil3d根据采样线进行填挖方计算。

右键道路模型【特性】调出道路模型特性对话框，在这里根据道路模型创建道路曲面。操作顺序见下图。

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-53-08.png"></div>

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-53-15.png"></div>

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-53-22.png"></div>

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-53-28.png"></div>

最后点击确定即可创建道路曲面，这时【曲面】下面会包含类似于下图所示的曲面对象。

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-53-37.png"></div>

下面创建采样线，在【常用】选项卡中点击【采样线】如下图： 

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-53-46.png"></div>

随后屏幕会出现一个拾取框，这时按一下空格键，会弹出从列表选择路线对话框，然后选择要采样的路线，点击确定，弹出【创建采样线编组】对话框，设置如下： 

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-53-54.png"></div>

点击确定后，【采样线工具】条被激活，如下图：

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-54-01.png"></div>

点击【按桩号范围...】进行创建采样线： 

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-54-09.png"></div>

点击后弹出如下对话框，设置如下，需要注意的是采样线的宽度，当道路模型比较宽时，采样线也要设置的宽一些。

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-54-17.png"></div>

点击确定按钮，同时关闭【采样线工具】条。创建的采样线如下图，这时注意左侧的目录树中的【路线】--【路线中线】--“道路中心线”--【采样线编组】下会显示刚刚创建的采样线编组，在这就可以看出采样线是路线下的子对象，采样线依附于路线，也就是说路线是采样线的容器。一个路线下面可以同时创建多个独立的采样线编组。 

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-54-26.png"></div>

最后一步就是计算填挖方量（在civil中叫计算材质ComputeMaterials）

首先拾取任意一个前面创建的采样线，菜单面板中会自动显示更采样线有关的操作，如下图：

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-54-33.png"></div>

点击【计算材质】命令后，跳出如下临时对话框，选择路线及采样线编组

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-54-41.png"></div>

点击确定后，跳出【计算材质】对话框，设置如下，需要注意的是自然地面使用地形曲面，设计曲面使用道路模型生成的曲面。

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-54-48.png"></div>

点击确定后，程序就会按照采样线设定的桩号间距根据平均断面面积进行计算，计算完成后采样线仍然处于选中状态，暂时不要按ESC键。 

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-54-55.png"></div>

点击上图中左侧的【添加表】---【总体积】命令，弹出如下对话框： 

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-55-06.png"></div>

最后单击确定按钮，在空白处插入总体积表，如下：

<div style="text-align:center;"><img src="/img/2022/2022-09-18-09-55-14.png"></div>

到此填挖方计算的基本步骤已经讲完，这个过程需要多次使用，才能体会其中的细节。还有就是上面的总体积表的显示样式也是可以定制的，有兴趣的可以先摸索一下，后面也会有介绍的。谢谢，祝大家开心。

转载自：[沙漠骆驼]（https://www.cnblogs.com/whlkx/）