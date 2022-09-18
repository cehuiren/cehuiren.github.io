---
layout: post
title:  "惠普HP-Prime可编程科学计算器测量程序代码-心墙下游反I与砾石土分界线放样"
date: 2015-05-23
categories: 编程开发 计算器
tags: 放样 HP-Prime 计算器程序
excerpt: 惠普HP-Prime可编程科学计算器支持纯文本格式程序代码，电脑端与计算器端传送代码简单方便。本代码是两河口水电站大坝心墙、垫层料、反滤料边线放样程序。
author: QinDong
---
* content
{:toc}

惠普HP-Prime可编程科学计算器支持纯文本格式程序代码，电脑端与计算器端传送代码简单方便。本代码是两河口水电站大坝心墙、垫层料、反滤料边线放样程序。代码中Points为表名，在计算器首页将表格保存命名为Points，以Points开头的代码均为从相应表中单元格读写数据。本代码为两河口水电站大坝心墙下游反I与砾石土分界线放样程序。

```vb
EXPORT XF1J()
BEGIN
LOCAL ch,X,Y,Z;
//下游左侧砼边线控制点A、B
LOCAL XLTA_X,XLTA_Y,XLTA_H,XLTB_X,XLTB_Y,XLTB_H;
//下游右侧砼边线控制点A、B
LOCAL XRTA_X,XRTA_Y,XRTA_H,XRTB_X,XRTB_Y,XRTB_H;
//下游左侧反1折线控制点A、B
LOCAL XLFA_X,XLFA_Y,XLFA_H,XLFB_X,XLFB_Y,XLFB_H;
//下游右侧反1折线控制点A、B
LOCAL XRFA_X,XRFA_Y,XRFA_H,XRFB_X,XRFB_Y,XRFB_H;

325.945▶XLTA_X;
71.5▶XLTA_Y;
2581.0▶XLTA_H;

324.145▶XLTB_X;
71.012▶XLTB_Y;
2583▶XLTB_H;

372.055▶XRTA_X;
71.5▶XRTA_Y;
2581.0▶XRTA_H;

373.855▶XRTB_X;
71.012▶XRTB_Y;
2583.0▶XRTB_H;

CHOOSE(ch,"下游反滤1与砾石土边界线放样","下游-左侧-砼边线","下游-左侧-反1折线",
"下游-右侧-砼边线","下游-右侧-反1折线","下游-反1砾石界线");
MSGBOX("覃东两河口水电站");

if ch==1 then
INPUT({X,Y,Z},"请输入测点坐标",{"桩号","偏距","高程"},{"桩号","偏距","提示"},{0,0,0})

end;

INPUT(X,"汉字");
23▶Y;
132▶Z
END;
```