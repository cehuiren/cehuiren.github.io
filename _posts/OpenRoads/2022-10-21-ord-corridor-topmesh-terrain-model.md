---
title: OpenRoads Designer公路廊道顶层网格生成及对应设计地形模型
author: QinDong
date: 2022-10-21 09:57:00 +0800
categories: OpenRoads 道路廊道
tags: 廊道 道路 网格 地形 方量
excerpt: 
---
* content
{:toc}

道路（廊道）创建后可以生成道路对应的顶层网格（边坡、路面等断面模板顶部连线成形的网格，路面以下各层需生成网格的在断面模板中各层设置备选表面特征名称），并由顶层网格生成道路设计地形模型。

选中道路（廊道）查看廊道属性中设置的阶段（一般为设计Design、和最终Final两个阶段，Design阶段侧重于工作效率，Final侧重于成果质量）：

![](/img/2022/2022-10-21-10-04-42.png)

![](/img/2022/2022-10-21-10-05-13.png)

在Explorer中找到Design特征：

![](/img/2022/2022-10-21-10-06-11.png)

修改属性中的Top Mesh Display属性为True:

![](/img/2022/2022-10-21-10-08-03.png)

对道路进行更新：

![](/img/2022/2022-10-21-10-08-55.png)

在层显示对话框中关闭所有层，打开网格所在层：

![](/img/2022/2022-10-21-10-10-19.png)

道路对应的设计网格生成：

![](/img/2022/2022-10-21-10-14-03.png)

可由此顶层网格按元素创建地形模型生成道路开挖设计地形模型：

![](/img/2022/2022-10-21-10-18-25.png)

进而由两期地形模型分析道路开挖总方量。