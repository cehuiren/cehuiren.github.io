---
layout: post
title:  "MicroStation导入Civil 3D地形曲面并转换成智能曲面"
date: 2022-09-02
categories: MicroStation 曲面
tags: MicroStation Civil3D Surface
excerpt: 利用Civil 3D强大的地形曲面创建和编辑功能进行大型地形处理编辑，导出为交换格式LandXML，在MicroStation内导入后转换为曲面或网格，用于三维建模时裁剪和创建封闭网格进行体积方量计算。
author: QinDong
---
* content
{:toc}

1、在C3D里生成曲面，调整好三角网，在OUTPUT选项卡里导出为LandXML文件；

2、在MSTN里导入LandXML，选中导入后生成的对象修改属性将三角网打开显示（如已显示可忽略此步）；

3、将三角网Drop成网格，参数如下：

![](/img/2022/2022-09-03-08-25-10.png)

4、在曲面中的曲面应用选择Convert to SmartSurfae将三角网格转换成SmartSurface：

![](/img/2022/2022-09-03-08-25-20.png)

注意：对于较大的地形，在转换后出现丢失部位的情况与SolidArea大小有关，在设置中调大此参数后就可以了，调整此参数后的地形在被引用时主图的大小需与此一致，否则无法裁剪。

至此C3D里的曲面三角网就生成了SmartSurface，参考到建模中即可用于裁剪其它曲面，无需并入主文件。

如果需要调整显示透明度，请将此曲面的透明度设为40左右，参考时在参考对话框内将此参考的设置中较正颜色：

![](/img/2022/2022-09-03-08-25-30.png)

其中的透明度设为1即可，大于此值会导致透明度太大，设置为0则无法让被参考模型内的透明度产生效果。