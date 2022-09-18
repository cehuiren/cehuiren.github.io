---
layout: post
title:  "惠普HP-Prime可编程科学计算器测量程序代码-控制点表中读出点坐标"
date: 2015-05-18
categories: 编程开发 计算器
tags: 控制点 HP-Prime 计算器程序
excerpt: 惠普HP-Prime可编程科学计算器支持纯文本格式程序代码，电脑端与计算器端传送代码简单方便。本代码是两河口水电站大坝心墙、垫层料、反滤料边线放样程序。
author: QinDong
---
* content
{:toc}

惠普HP-Prime可编程科学计算器支持纯文本格式程序代码，电脑端与计算器端传送代码简单方便。本代码是两河口水电站大坝心墙、垫层料、反滤料边线放样程序。代码中Points为表名，在计算器首页将表格保存命名为Points，以Points开头的代码均为从表中单元格读入数据。本代码为从控制点表Points中将点坐标读出赋值给变量。

```vb
EXPORT 读取已知点()
BEGIN
LOCAL i,ch,pointNames:=Points.A:A;
LOCAL pointN:=Points.B:B;
LOCAL pointE:=Points.C:C;
LOCAL pointH:=Points.D:D;

CHOOSE(ch,"请选择一个已知点",pointNames);

IF ch>0 THEN
PRINT();
PRINT("点名:"+pointNames[ch]);
PRINT("N:"+pointN[ch]);
PRINT("E:"+pointE[ch]);
PRINT("H:"+pointH[ch]);
END;
END;
```