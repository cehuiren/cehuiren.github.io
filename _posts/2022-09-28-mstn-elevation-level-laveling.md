---
title: MicroStation高程标高标注
author: QinDong
date: 2022-09-28 19:58:00 +0800
categories: MicroStation 基础
tags: 高程 标注
excerpt: 
---
* content
{:toc}


## 标高标注
MS中带有一个专门用于标注标高的工具，名字叫做Ordinate Dimensioning。对于MSCE来说，可从如下位置找到它：

![](/img/2022/2022-09-28-19-58-32.png)

该工具使用起来稍有令人迷惑的地方，主要就是需要明确各个点的含义。下面详述操作步骤：

点击该图标后出现如下图所示的工具设置框，其中的Datum Value默认值为0，表示你基准线的高程。

![](/img/2022/2022-09-28-19-58-38.png)

然后你在当前视图中指定一个点为基准点，然后第二个点定义标注高程的方向，一般就是冲上，当然也可以沿着水平方向去标注。第三个点为标注基准线的终点，而后的第四、第五等等点则为真正你想要标注的高程位置的点。如下图所示来指定各个点的位置：

![](/img/2022/2022-09-28-19-58-43.png)

## 高程标注
MicroStation带有坐标标注工具，默认是标注X、Y、Z，可以通过键入命令关闭平面坐标X、Y的显示，只标注Z坐标，键入命令为：`label hidexy on`，
若要重新打开三维坐标标注，输入命令`label hidexy off`即可。

![](/img/2022/2022-09-28-20-07-59.png)