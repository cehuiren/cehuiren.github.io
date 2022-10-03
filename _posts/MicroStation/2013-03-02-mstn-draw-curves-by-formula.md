---
title: MicroStation由数学公式绘制曲线
author: 中国优先社区
date: 2013-02-28 09:28:00 +0800
categories: MicroStation 基础
tags: 曲线 公式
excerpt: 
---
* content
{:toc}

MicroStation中带有一个强大的工具，那就是根据数学公式绘制曲线。您可以从预定义的曲线库中选择公式，也可以自定义公式。

可以用三角函数、双曲函数、指数函数、对数函数以及乘方函数来创建正弦曲线(sinusoid)、螺旋线(spiral)、悬链线(catenary)、渐开线(involute)和渐屈线(evolute)等。

该工具位于Surface Modeling任务类别下的Curve Utilities工具栏中。如下图所示：

![](/img/2022/2022-09-30-20-28-41.png)

点击上图中的工具后会弹出一个Curve by Formula对话框，点击该对话框中的菜单File > Open File，在...\Workspace\System\data文件夹下（默认就应该在此文件夹下）找到curve.rsc文件打开。如下图所示：

![](/img/2022/2022-09-30-20-29-05.png)

系统所带曲线库及其内容如下表所示：

库名称 | 函数英文描述 | 对应中文含义
--- | --- | ---
curve.rsc | Line length &angle | 根据长度和角度绘制一条线
... | Elliptical arc | 椭圆弧
...| General quadratic | 通用二次曲线
... | General cubic | 通用三次曲线
... | Logarithmic | 对数曲线
... | Offset | 偏移曲线
... | Evolute | 渐屈线
... | Catenary by points | 根据点生成悬链线
... | Gaussian distribution | 高斯分布（即正态分布）曲线
... | Line at angle | 根据弧度绘制直线
curve3d.rsc | Elliptical helix | 弧螺旋线
... | Conical helix | 圆锥螺旋线
... | Toroidial spiral | 环形螺旋线
cycloid.rsc | Cycloid | 圆摆线
... | Trochoid | 次摆线
... | Epicycloid | 外摆线
... | Hypocycloid | 内摆线
spiral.rsc | Archimedes spiral | 阿基米德曲线
... | Logarithmic spiral | 对数曲线
... | Involute of circle | 圆的渐屈线
... | Clothoid spiral | 回旋曲线
... | Transition spiral,degree | 过渡线（以度作为参数）

【注】：在Open Curve Resource对话框的Files下可选择以上四个曲线库之一，其他的rsc文件并不是曲线库（它们都是以.rsc结尾的，属于Mstn中的一种资源，但不是曲线库资源）。

### 如何放置曲线库中的一条曲线？
1、在如上图所示的Open Curve Resource对话框中选择某个曲线库，然后再选择某种曲线；

2、查看各种设置是否合适，不合适的话可按自己的需要来修改。比如，要以B样条曲线放置（CreateAs = Bspline）还是以线串放置（CreateAs=Line String）；按定义放置（Mode = Defined）还是根据已有曲线推导出新的曲线（Mode = Derived）；所用角度是弧度（Angle = Radian）还是度（Angle = Degree）。

3、点击Place按钮，在视图中点一点放置即可。

### 如何自定义一条曲线？
1、在Curve by Formula对话中选菜单File > New Curve启动创建新曲线的功能；

2、在Name中输入新曲线的名称，如TestCurve；

3、在下面的空白区输入公式定义。如我们要定义一个最简单的正弦函数曲线y=sin(x)。则可如下写：

![](/img/2022/2022-09-30-20-29-39.png)

请注意，在公式定义中有几个字母具有特殊含义，x，y，z表示曲线上的坐标点，t表示函数参数，它是一个从0到1的实数。公式定义只能生成10个点构成的一条曲线。

我们要形成完整的一个正弦周期，所以对x的赋值为2*pi*t，对y的赋值就是sin(x)。注意每行要用分号结束。这样的定义就能绘制出一条完美的正弦曲线。

4、选File > Save 或File > Save To将自定义曲线保存的某个rsc的曲线库中。Save是在当前有打开的曲线库的前提下用，Save To是保存到其他的一个曲线库中。

公式定义中用到常量（pi和e）以及标准函数可从菜单Insert下找到。

### 派生曲线的生成
派生曲线就是先指定一条已有曲线，然后根据定义的公式生成一条推导出的曲线。
要学习派生曲线需要先了解如下预定义项：

值 | 描述
--- | ---
_rx, _ry, _rz | 根曲线（即现有曲线）的位置坐标
_tx, _ty, _tz | 根曲线的切线坐标
_mx, _my, _mz | 根曲线的法线坐标
_bx, _by, _bz | 根曲线的二次法线坐标
_kappa | 根曲线的曲率
_tau | 根曲线的扭矩
 
在curve.rsc曲线库中有Offset公式就是定义的一个偏移量为-5个主单位的派生曲线。其公式定义如下：

![](/img/2022/2022-09-30-20-29-51.png)

可以看到新生成曲线的x、y、z坐标是以原有曲线的x、y、z坐标（_rx、_ry、_rz）为基础，又加上了其法线坐标（_mx、_my、_mz）乘以距离。

注意，Mode要选择Derived（派生），此时，原有的Place（放置）按钮也会变成Construct（构造）。