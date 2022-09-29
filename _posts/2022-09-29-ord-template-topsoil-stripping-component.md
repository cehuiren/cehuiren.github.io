---
title: OpenRoads模板清除表土组件设置
author: QinDong
date: 2022-09-29 09:38:00 +0800
categories: OpenRoads 断面模板
tags: 组件 表土 土方 模板
excerpt: 
---
* content
{:toc}

1、进入模板设置

![](/img/2022/2022-09-29-14-04-19.png)

2、添加overlay/Stripping组件

![](/img/2022/2022-09-29-14-05-09.png)

3、设置组件参数：
特征设为Mesh\Grading\TC_Topsoil，Top option和Bottom option均设置为Follow Surface，Surface Depth按清除表土厚度值设定，标签按规则命名。

![](/img/2022/2022-09-29-14-06-44.png)

4、为路面组件和放坡末端条件逐一设置覆盖组件:
![](/img/2022/2022-09-29-14-07-32.png)

5、将Topsoil Stripping组件的父组件设置为各放坡末端条件组件:
![](/img/2022/2022-09-29-14-21-24.png)

![](/img/2022/2022-09-29-14-22-15.png)

6、测试组件是否符合预期:
![](/img/2022/2022-09-29-14-22-38.png)

6、廊道(道路)模型和剖面效果:
![](/img/2022/2022-09-29-14-23-26.png)

>Video:Topsoil Stripping With Components
{: .promp-info}