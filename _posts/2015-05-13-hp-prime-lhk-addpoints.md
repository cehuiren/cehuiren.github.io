---
layout: post
title:  "惠普HP-Prime可编程科学计算器测量程序代码-添加控制点表"
date: 2015-05-15
categories: 编程开发 计算器
tags: 控制点 HP-Prime 计算器程序
excerpt: 惠普HP-Prime可编程科学计算器支持纯文本格式程序代码，电脑端与计算器端传送代码简单方便。本代码是两河口水电站大坝心墙、垫层料、反滤料边线放样程序。
author: QinDong
---
* content
{:toc}

惠普HP-Prime可编程科学计算器支持纯文本格式程序代码，电脑端与计算器端传送代码简单方便。本代码是两河口水电站大坝心墙、垫层料、反滤料边线放样程序。代码中Points为表名，在计算器首页将表格保存命名为Points，以Points开头的代码均为向表中单元格写入数据。代码中示例数据是将测量控制点坐标写入表格，方便程序和现场调用。

```vb
EXPORT 控制点()
BEGIN
Points.A1:="DB01";
Points.B1:=3743706.357;
Points.C1:=268916.356;
Points.D1:=2659.461;

Points.A2:="DB02";
Points.B2:=3743548.524;
Points.C2:=268610.543;
Points.D2:=2764.479;

Points.A3:="DB03";
Points.B3:=3743400.114;
Points.C3:=268599.882;
Points.D3:=2764.737;

Points.A4:="DB04";
Points.B4:=3743088.292;
Points.C4:=268638.100;
Points.D4:=2673.991;

Points.A5:="III14";
Points.B5:=3742787.169;
Points.C5:=268728.4778;
Points.D5:=2658.436;

Points.A6:="II17";
Points.B6:=3742702.4638;
Points.C6:=268672.7797;
Points.D6:=2762.770;

Points.A7:="LIII37";
Points.B7:=3745684.856;
Points.C7:=247946.293;
Points.D7:=2802.464;

Points.A8:="LIII39";
Points.B8:=3746535.540;
Points.C8:=246709.521;
Points.D8:=2846.699;

Points.A9:="II22";
Points.B9:=3743924.1343;
Points.C9:=269080.5565;
Points.D9:=2784.724;

Points.A10:="II11";
Points.B10:=3744094.454;
Points.C10:=269230.420;
Points.D10:=2882.874;

Points.A11:="II06";
Points.B11:=3742513.0166;
Points.C11:=269351.6005;
Points.D11:=2920.656;

Points.A12:="II04";
Points.B12:=3742229.1747;
Points.C12:=269430.2706;
Points.D12:=3012.752;

Points.A13:="II14";
Points.B13:=3742696.6192;
Points.C13:=269332.7309;
Points.D13:=2916.646;

Points.A14:="II09";
Points.B14:=3744023.3003;
Points.C14:=268488.0585;
Points.D14:=2988.623;

Points.A15:="III29";
Points.B15:=3744183.570;
Points.C15:=268685.282;
Points.D15:=2790.267;
END;
```