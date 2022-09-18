---
layout: post
title:  "惠普HP-Prime可编程科学计算器测量程序代码-添加坐标系表"
date: 2015-05-13
categories: 编程开发 计算器
tags: 计算器 坐标系 可编程 HP-Prime 计算器程序
excerpt: 惠普HP-Prime可编程科学计算器支持纯文本格式程序代码，电脑端与计算器端传送代码简单方便。本代码是两河口水电站大坝心墙、垫层料、反滤料边线放样程序。
author: QinDong
---
* content
{:toc}

惠普HP-Prime可编程科学计算器支持纯文本格式程序代码，电脑端与计算器端传送代码简单方便。本代码是两河口水电站大坝心墙、垫层料、反滤料边线放样程序。代码中CoSys为表名，在计算器首页将表格保存命名为CoSys，以CoSys开头的代码均为向表中单元格写入数据。代码中示例数据是工程坐标系定义数据。

```vb
EXPORT 施工坐标系()
BEGIN
//施工坐标系名
//A点坐标N
//A点坐标E
//A点施工坐标桩号
//A点施工坐标偏距
//B点坐标N
//B点坐标E
CoSys.A1:="A2B2";
CoSys.B1:=3743173.790;
CoSys.C1:=268789.559;
CoSys.D1:=294.0;
CoSys.E1:=0;
CoSys.F1:=3743173.790;
CoSys.G1:=268679.559;

CoSys.A2:="AB";
CoSys.B2:=3743173.790;
CoSys.C2:=269083.559;
CoSys.D2:=0;
CoSys.E2:=0;
CoSys.F2:=3743173.790;
CoSys.G2:=268415.559;

CoSys.A3:="SY1SY2";
CoSys.B3:=3743720.357;
CoSys.C3:=268922.576;
CoSys.D3:=0;
CoSys.E3:=0;
CoSys.F3:=3743715.889;
CoSys.G3:=268759.6478;

CoSys.A4:="SY3SY4";
CoSys.B4:=3743699.972;
CoSys.C4:=268930.806;
CoSys.D4:=0;
CoSys.E4:=0;
CoSys.F4:=3743620.876;
CoSys.G4:=269007.8794;
END;
```
