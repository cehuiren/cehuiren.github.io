---
layout: post
title:  "惠普HP-Prime可编程科学计算器测量程序代码-两河口心墙下游反滤料边线放样"
date: 2015-05-20
categories: 编程开发 计算器
tags: 计算器 放样 可编程 HP-Prime 计算器程序
excerpt: 惠普HP-Prime可编程科学计算器支持纯文本格式程序代码，电脑端与计算器端传送代码简单方便。本代码是两河口水电站大坝心墙、垫层料、反滤料边线放样程序。
author: QinDong
---
* content
{:toc}

惠普HP-Prime可编程科学计算器支持纯文本格式程序代码，电脑端与计算器端传送代码简单方便。本代码是两河口水电站大坝心墙、垫层料、反滤料边线放样程序。代码中Points为表名，在计算器首页将表格保存命名为Points，以Points开头的代码均为从相应表中单元格读写数据。本代码为两河口水电站大坝心墙下游反滤料I边线放样。

```vb
EXPORT 下游反1界线放样()
BEGIN
LOCAL pax,pay,pah,pbx,pby,pbh;
LOCAL px,py,ph,ch;
LOCAL tmpx,tmpy,dltx,dlty;

PRINT();
PRINT(" ");
PRINT(" ");
PRINT(" 程序测试中，计算结果请认真与现场核对如有不符");
PRINT(" 请立即停止使用并与我联系：TEL 139 66221102");
PRINT(" ");
PRINT(" 使用中将测点坐标任一值输为-1退出，按回车开始");
PRINT(" ");
PRINT(" ");
PRINT(" ");
PRINT("    中国电建一二•五联合体测量队 Q.D. 2016.10");
WAIT();

REPEAT

INPUT({px,py,ph},"请输入测量点坐标：",
{"测点桩号S：","测点偏距Y：","测点高程H："},
{"测点桩号S：","测点偏距Y：","测点高程H："});

CHOOSE(ch,"请选择需要计算的对象","左岸砼盖板边线点","左岸接触粘土界线点","左岸放坡(1:0.75)折线点","EL.2583以下1:0.75边坡放样","右岸放坡(1:0.75)折线点","右岸接触粘土界线点","右岸砼盖板边线点");

CASE
IF ch==1 THEN
pax:=325.945;
pay:=71.5;
pah:=2581;

pbx:=324.145;
pby:=71.012;
pbh:=2583;

tmpx:=finvalue(pah,pax,pbh,pbx,ph);
tmpy:=finvalue(pah,pay,pbh,pby,ph);

dltx:=px-tmpx;
dlty:=py-tmpy;

PRINT();
PRINT("您输入的测点坐标是：S="+ROUND(px,3)+" Y="+ROUND(py,3)+" H="+ROUND(ph,3));
PRINT("测点高程"+ROUND(ph,3)+"对应的左岸砼边线坐标为：");
PRINT("S="+ROUND(tmpx,3)+", Y="+ROUND(tmpy,3));
PRINT("测点与对应的左岸砼盖板边线点坐标差值： dltS="+ROUND(dltx,3)+", dltY="+ROUND(dlty,3));

PRINT(" ");
PRINT("中国电建一二•五联合体测量队 Q.D. 2016.10");
END;

IF ch==2 THEN
//左岸接触粘土界线
pax:=329.945;
pay:=71.5;
pah:=2581;

pbx:=328.145;
pby:=71.012;
pbh:=2583;

tmpx:=finvalue(pah,pax,pbh,pbx,ph);
tmpy:=finvalue(pah,pay,pbh,pby,ph);

dltx:=px-tmpx;
dlty:=py-tmpy;

PRINT();
PRINT("您输入的测点坐标是：S="+ROUND(px,3)+" Y="+ROUND(py,3)+" H="+ROUND(ph,3));
PRINT("测点高程"+ROUND(ph,3)+"对应的左岸接触粘土界线坐标为：");
PRINT("S="+ROUND(tmpx,3)+", Y="+ROUND(tmpy,3));
PRINT("测点与对应的左岸接触粘土界线点坐标差值： dltS="+ROUND(dltx,3)+", dltY="+ROUND(dlty,3));

PRINT(" ");
PRINT("中国电建一二•五联合体测量队 Q.D. 2016.10");
END;

IF ch==3 THEN
//左岸放坡线折线
pax:=329.945;
pay:=71.5;
pah:=2581.0;

pbx:=332.205;
pby:=70.0;
pbh:=2583.0;

tmpx:=finvalue(pah,pax,pbh,pbx,ph);
tmpy:=finvalue(pah,pay,pbh,pby,ph);

dltx:=px-tmpx;
dlty:=py-tmpy;

PRINT();
PRINT("您输入的测点坐标是：S="+ROUND(px,3)+" Y="+ROUND(py,3)+" H="+ROUND(ph,3));
PRINT("测点高程"+ROUND(ph,3)+"对应的1:0.75放坡左岸折线坐标为：");
PRINT("S="+ROUND(tmpx,3)+", Y="+ROUND(tmpy,3));
PRINT("测点与对应的折线点坐标差值： dltS="+ROUND(dltx,3)+", dltY="+ROUND(dlty,3));

PRINT(" ");
PRINT("中国电建一二•五联合体测量队 Q.D. 2016.10");
END;

IF ch==4 THEN
pax:=329.945;
pay:=71.5;
pah:=2581.0;

pbx:=332.205;
pby:=70.0;
pbh:=2583.0;

tmpx:=finvalue(pah,pax,pbh,pbx,ph);
tmpy:=finvalue(pah,pay,pbh,pby,ph);

dltx:=px-tmpx;
dlty:=py-tmpy;

PRINT();
PRINT("您输入的测点坐标是：S="+ROUND(px,3)+" Y="+ROUND(py,3)+" H="+ROUND(ph,3));
PRINT("测点高程"+ROUND(ph,3)+"对应的1:0.75边坡偏距为：");
PRINT("Y="+ROUND(tmpy,3));
PRINT("测点与对应的坡边线点坐标差值： dltY="+ROUND(dlty,3));

PRINT(" ");
PRINT("中国电建一二•五联合体测量队 Q.D. 2016.10");
END;

IF ch==5 THEN
pax:=368.055;
pay:=71.5;
pah:=2581;

pbx:=365.805;
pby:=70.0;
pbh:=2583.0;

tmpx:=finvalue(pah,pax,pbh,pbx,ph);
tmpy:=finvalue(pah,pay,pbh,pby,ph);

dltx:=px-tmpx;
dlty:=py-tmpy;

PRINT();
PRINT("您输入的测点坐标是：S="+ROUND(px,3)+" Y="+ROUND(py,3)+" H="+ROUND(ph,3));
PRINT("测点高程"+ROUND(ph,3)+"对应的1:0.75放坡右岸折线坐标为：");
PRINT("S="+ROUND(tmpx,3)+", Y="+ROUND(tmpy,3));
PRINT("测点与对应的折线点坐标差值： dltS="+ROUND(dltx,3)+", dltY="+ROUND(dlty,3));

PRINT(" ");
PRINT("中国电建一二•五联合体测量队 Q.D. 2016.10");
END;

IF ch==6 THEN
pax:=368.055;
pay:=71.5;
pah:=2581.0;

pbx:=369.855;
pby:=71.012;
pbh:=2583.0;

tmpx:=finvalue(pah,pax,pbh,pbx,ph);
tmpy:=finvalue(pah,pay,pbh,pby,ph);

dltx:=px-tmpx;
dlty:=py-tmpy;

PRINT();
PRINT("您输入的测点坐标是：S="+ROUND(px,3)+" Y="+ROUND(py,3)+" H="+ROUND(ph,3));
PRINT("测点高程"+ROUND(ph,3)+"对应的右岸接触粘土界线坐标为：");
PRINT("S="+ROUND(tmpx,3)+", Y="+ROUND(tmpy,3));
PRINT("测点与对应的右岸接触粘土界线点坐标差值： dltS="+ROUND(dltx,3)+", dltY="+ROUND(dlty,3));

PRINT(" ");
PRINT("中国电建一二•五联合体测量队 Q.D. 2016.10");
END;

IF ch==7 THEN
pax:=372.055;
pay:=71.5;
pah:=2581.0;

pbx:=373.855;
pby:=71.012;
pbh:=2583.0;

tmpx:=finvalue(pah,pax,pbh,pbx,ph);
tmpy:=finvalue(pah,pay,pbh,pby,ph);

dltx:=px-tmpx;
dlty:=py-tmpy;

PRINT();
PRINT("您输入的测点坐标是：S="+ROUND(px,3)+" Y="+ROUND(py,3)+" H="+ROUND(ph,3));
PRINT("测点高程"+ROUND(ph,3)+"对应的右岸砼边线坐标为：");
PRINT("S="+ROUND(tmpx,3)+", Y="+ROUND(tmpy,3));
PRINT("测点与对应的右岸砼盖板边线点坐标差值： dltS="+ROUND(dltx,3)+", dltY="+ROUND(dlty,3));

PRINT(" ");
PRINT("中国电建一二•五联合体测量队 Q.D. 2016.10");
END;

DEFAULT
px:=-1;
py:=-1;
ph:=-1;
END;

WAIT();

UNTIL px==-1 OR py==-1 OR ph==-1;

END;
```