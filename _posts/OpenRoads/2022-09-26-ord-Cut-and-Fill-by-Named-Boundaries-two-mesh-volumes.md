---
title: OpenRoads两期工程量(挖填)方量计算
author: QinDong
date: 2022-09-26 20:56:00 +0800
categories: OpenRoads 体积方量
tags: 挖填方 工程量 方量
excerpt: 
---
* content
{:toc}

- 分别生成各期地形，由Create Terrain Model From Ascii File展点；

![](/img/2022/2022-09-26-21-07-26.png)

- 其中至少一期地形特征设为Existing组特征

- 创建挖填体积（Terrain\Analysis\Create Cut Fill Volumes或Home\Model Analysis and Reporting\Civil Analysis\Create Cut Fill Volumes）；

![](/img/2022/2022-09-26-21-07-38.png)

- 查看挖填方量报表（Home\Model Analysis and Reporting\Civil Analysis\Quantities Report By Names Boundary）,直接点击确定，命名边界选无；

![](/img/2022/2022-09-26-21-07-49.png)

![](/img/2022/2022-09-26-21-07-55.png)

- 也可直接在分析地形模型的体型（Terrain\Analysis\Analyze Volume）,选地模至地模方式。

![](/img/2022/2022-09-26-21-08-03.png)

![](/img/2022/2022-09-26-21-08-09.png)

- 如果想划定区域计算方量，则可以用两点方式添加命名边界（Drawing Production\Named Boundaries），再统计命名边界体积报表：

![](/img/2022/2022-09-26-21-08-20.png)
	
创建挖填方体后可以直接查询体积块网格的体积属性，也可以使用Home\Model Anaslysis and Reporting\Civil Analysis\Quantities Report By Named Boundary即命名边界工程量报表，提示选择命名边界时，选择None（无）即可。生成的报告中含挖填方量。

工具位于：Home\Model Anaslysis and Reporting\Civil Analysis\Quantities Report By Named Boundary

模型中必须存在填挖方体。

[create cut fill volumes](https://www.youtube.com/watch?v=ycso0_ir6bE&ab_channel=CivilTSG)

[Cut and Fill by Named Boundaries](https://www.youtube.com/watch?v=yVlA9psPJrY&ab_channel=BentleySystemsHongKong)

单个廊道：[3_根据命名边界出工程量表 (bentley-learn.com)](https://bentley-learn.com/detail/v_61699eeee4b0efd5af4a1c1a/3)

多个廊道：[4_多个廊道工程量 (bentley-learn.com)](https://bentley-learn.com/detail/v_61699f2de4b025ffb25ecd99/3)

[创建填挖方体_1_Existing Design Subgrade None](https://bentley-learn.com/detail/v_61698d04e4b0efd5af4a1914/3)

[创建填挖方体_2_非适用性材料](https://bentley-learn.com/detail/v_61698e47e4b0b71b3b595103/3)

[创建填挖方体_3_回填](https://bentley-learn.com/detail/v_61698f2ce4b025ffb25ecb08/3)

[创建填挖方体_4_超挖](https://bentley-learn.com/detail/v_6169905ae4b025ffb25ecb6f/3)

[创建填挖方体_5_地层（土石方、土石分界、土方、石方）](https://bentley-learn.com/detail/v_61699136e4b025ffb25ecba3/3)

[Earthwork Quantities in OpenRoads Designer](https://www.youtube.com/watch?v=I-eAcHdllLc&t=312s&ab_channel=BentleyOpenRoads)

[Earthwork Quantities in OpenRoads Designer_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1TF41167fq?spm_id_from=333.999.0.0)

[Session 6 - Computing Earthwork and Generating Reports](https://www.youtube.com/watch?v=IDW_E4u7Sls&t=27s&ab_channel=BentleyOpenRoads)

[Module 5 - Advanced 3D Volumes & Earthwork](https://www.youtube.com/watch?v=TQR28fMKtvo&ab_channel=BentleyOpenRoads)

[Module 3 - 3D Volumes & Earthwork](https://www.youtube.com/watch?v=ycxB__CEo6Y&ab_channel=BentleyOpenRoads)