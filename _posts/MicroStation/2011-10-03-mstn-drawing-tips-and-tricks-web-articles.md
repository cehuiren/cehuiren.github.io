---
title: MicroStation绘图操作总结（网文摘录）
author: QinDong
date: 2011-10-03 15:53:00 +0800
categories: MicroStation 基础
tags: ACS AccuDraw 视图
excerpt: 
---
* content
{:toc}

1.项目管理的原则：不能直接双击打开Dgn文件，要先选定环境，可以先file->close,再选择环境

2.五级环境变量：系统级 应用程序级 公司级 项目级 个人级，若有重复，则最终取决于个人级

3.每一个工具都有一个属性块来管理

4.快捷键：

F11:选定精确坐标

回车:锁定此时画线所在的坐标轴

x:使x值不可变

y:使y值不可变

a:锁定角度

空格:切换画线方式（有垂直坐标和极坐标两种方式）

V:使坐标系与视图平齐

Page Down:切换到前一次的值

Page Up:切换到后一次的值

精确绘图可做+ - * /运算 

o：将精确绘图坐标转移到捕捉到的点

k:设置线段的可捕捉点为几等分点

i:临时捕捉两线段交点 c:临时捕捉图形中心点 n:临时捕捉最近点

RQ:用于锁定一定的角度

~:可用于切换命令的不同形式

Ctrl+z:后退 Ctrl+r:前进

q:退出精确绘图

d:锁定距离

a:锁定角度

?:显示快捷键

5.View Attributes 快捷键:Ctrl+b和F5（临时性的）

Saved Views

Views:1-6_1,各种视图工具

Drawing:1-7_1,1-7_2 各种画图形命令

Tasks_3(Manipulate:Copy,Move,Move Parallel,Rotate...):1-7_3 Tasks_7(Modify):1-7_5


同时按下左键和右键可固定或者说暂停在任意一个点


两种颜色格式：CMYK（真彩色，用作印刷） RGB（用作电子显示） 


6.Place Arc:画圆弧

Break Element:去掉图形的一部分

Move Parallel 偏移（可改变其颜色，使其为当前选择的颜色或者说当前激活(Active)的颜色）

7.三维

Rotate View

Display Style:Illustration Smooth

Display(Shaded) Background(Color)


Settings->Dsign File Settings->Working Units:

Linear Units里面的精确度是显示的精确度

真正的精确度需在Advanced Settings里面设定


t:使坐标系与x-y平面平行 s:使坐标系与y-z平面平行 f:使坐标系与x-z平面平行（默认情况下与世界坐标系对齐，也可用一定的方法使其与ACS对齐）

e:使坐标系在三种方式间切换 RX RY RZ也可达到相同的效果


z:使z值不可变，即使点固定在一个平面上


三种坐标系：

世界坐标系：不可见，正中心即为世界坐标系的原点，使用P,M快捷键输入其坐标值

ACS：可见

精确坐标系：具体画图时所用的“尺子”


RA：重置ACS坐标系 WA:保存ACS GA:得到保存好的ACS 

LP：使TSF与当前ACS对齐


三维我们需要注意的原则：

1.面对空空的且无参照的View，利用TSF，知道自己的方向

2.移动，复制明确方向

3.回车-保证我们的方向

4.线是有方向的，面是有正反的，面是没有厚度的，只有体有厚度


Cut Solids by Curves:用曲线切固体

Solid by Extrusion:通过拉伸将面变成体（由Surface by Extrusion命令形成的面不可用此命令，此时可用Solid by Thicken Surface命令）

Solid by Thicken Surface:通过给面增加厚度拉伸成体（任何面都可用此命令）

Surface by Extrusion:通过拉伸将线变成面

Extract Faces/Edges:提取面或边

Fillet Edges:将面的边界形成圆角

Chamfer Edges:将面的边界形成斜角

Unite Solids:合并有交点的体

Shell Solid:抽壳

Solid by Extrusion Along:沿路径（曲线）将面拉伸成体


切固体时，切的对象和被切的对象不能有重合的部分

线（未封闭）拉伸成面，面（包括封闭的曲线）拉伸成体，即相当于给面增加了厚度



8.Replace Face:用一个面替换另一个面

Remove Entity by Size:

Delete Solid Entity:

camera Brightness