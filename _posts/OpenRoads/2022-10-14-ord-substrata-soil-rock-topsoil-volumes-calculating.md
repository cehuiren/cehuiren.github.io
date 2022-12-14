---
title: OpenRoads Designer分地层计算土、石方量
author: QinDong
date: 2022-10-12 16:48:00 +0800
categories: OpenRoads 体积方量
tags: 体积 方量 土方 石方 土石方 表土
excerpt: 
---
* content
{:toc}

在实际工作中土、石方分界通常按厚度、土方剥离后实测地形两种方式进行土、石方分界。在ORD中可实现分地层快速自动计算各地质层（表土、土方、石方）开挖方量。对于区域内既有指定厚度也有实测地形的，需要对原始地形进行剪切将指定厚度部分和实测部分划分开来，分别按下述的不同方法创建地质层。

## 创建地（质）层
1、生成原始地形文件（2D种子），从ASCII文件生成地形模型；

2、新建地质层模型文件（3D种子），参考原始地形文件，模型为Default-3D；

![](/img/2022/2022-10-14-10-04-19.png)

### 按厚度创建地层

OpenRoads Modeling工作流、Model Detailing、3D Tools、Create Closed Mesh

![](/img/2022/2022-10-14-10-06-21.png)

特征分别设置为：TC_Sandstone(表土)、TC_Limestone（土方）、TC_Rock（石方），生成方式选元素和厚度（如果有确度厚度范围线，可选元素、厚度及元素边界）

![](/img/2022/2022-10-14-10-06-35.png)

按提示选择原始地形模型（参考）、元素边界，确认各项参数后生成相应表土地模：

![](/img/2022/2022-10-14-10-17-45.png)

细部放大后效果：

![](/img/2022/2022-10-14-10-18-22.png)

重复此步生成土方地层和石方地层：

![](/img/2022/2022-10-14-10-20-27.png)

生成的石方地层要超过开挖厚度（即全范围深度大于收方地形）：

![](/img/2022/2022-10-14-10-25-48.png)

![](/img/2022/2022-10-14-10-24-50.png)

### 按两期地模（原始地形+实测土石分界地形）创建地层

对于土方实测地形，按以上步骤，在生成地层闭合网格时选元素与元素方法生成即可：

![](/img/2022/2022-10-14-10-27-43.png)

>需要注意的是，地层模型中的地形模型不参与体积计算，因此地层模型中各地模特征必须设定为Volume为None的特征（可将现有特征的体积选项设为无或新建一个自定义特征）。
{: .prompt-warning}

## 分层方量计算

### 创建方量计算文件

2D种子创建名为`方量计算.dgn`{: .filepath} 的文件。

### 参考相关文件

需要参考原始地形、收方地形、地层模型：

![](/img/2022/2022-10-14-10-38-10.png)

### 创建挖填方体

OpenRoad Modeling工作流：

Home\Model Analysis and Reporting\Civil Analysis\Create Cut Fill Volumes
或
Terrain\Analysis\Volumes\Create Cut Fill Volumes

![](/img/2022/2022-10-14-10-46-43.png)

>注意：与直接计算挖填方不同的是，在有地层模型的情况下，需要勾选“计算地层”选项。
{: .prompt-warning}

### 方量报表
#### 查询体积
最直接的方式是打开相应图层显示各地层开挖体积块并查询其体积属性：

![](/img/2022/2022-10-14-11-08-18.png)

![](/img/2022/2022-10-14-11-08-58.png)
_石方开挖区域_

![](/img/2022/2022-10-14-11-09-42.png)
_表土清除区域_

![](/img/2022/2022-10-14-11-10-21.png)
_土方开挖区域_

#### 方量表格
创建一条中心轴线（对无中心线的区域工程可以任定一条贯穿整个区域的直线当作中心轴线），按指定高程方式生成纵剖面，放置命名边界，生成端面积报表：

![](/img/2022/2022-10-14-15-03-36.png)
_任意直线作为中心线_

![](/img/2022/2022-10-14-15-04-05.png)
_按间距20米放置命名边界，即断面布置_

查询端面积报表：

![](/img/2022/2022-10-14-14-57-47.png)

在报表中查询相应地层体积：

![](/img/2022/2022-10-14-14-50-41.png)
_断面间距20米工程量报表_

![](/img/2022/2022-10-17-09-13-56.png)

![](/img/2022/2022-10-17-08-59-29.png)
_断面间距5米工程量报表_

当把断面间距调小到5米时，计算的表土、土方、石方总量7.19万方与总量相符。本示例中为几个开挖平台，平台间有数级陡坎，因此在断面间距为20米时报表中各地层开挖量相加与总量有几千方出入。报表中的Volumes_Cut为当初定义岩石地层时设定的厚度20米，局部最深处岩石地模型厚度未达到开挖厚度所致，此处差值较小，可忽略，实际工作中定义岩石地层厚度时尽量超出开挖深度。表中Volumes_Fill为收方地形超出原始地形范围以外的区域按填方进行计算的方量，无意义。

>因定义地层时厚度会远超开挖区域，因此报表中只需查看移除项（表中带removed的挖除方量）即可。地层总体积是整个地层体积，无意义可忽略。
{: .prompt-warning}

若使用以下方式查询，在提示选择命名边界时可以直接确定即选无即可。

Home\Model Analysis and Reporting\Civil Analysis\Quantities Report By Named Boundary直接查询各地层挖填方报表：

![](/img/2022/2022-10-14-10-50-52.png)