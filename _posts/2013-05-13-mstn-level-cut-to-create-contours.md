---
title: MicroStation水平剖切（生成等高线）
author: QinDong
date: 2013-05-03 13:18:00 +0800
categories: MicroStation 基础
tags: 剖切 等高线
excerpt: 
---
* content
{:toc}

1、选择平面切分工具（建模 > 曲面 > 曲面实用工具 >Planar Slice）。

![](/img/2022/2022-09-30-20-40-38.png)

2、单击按三个点切分图标（第二个图标）。

输入三个数据点以定义剖切平面（CE里按P再按M，对于宁海坐标通常为：363787.698、3253823.914,575区域的地形，可输入一个包含该地形区域的大范围如：0,0,590、400000,0,590、0,4000000,590）。

3、输入一个数据点以查看剖面元素。

4、 输入一个数据点以完成剖面。

水平剖切记录的相关宏：

```vb
Sub Bmrsection()
  Dim startPoint As Point3d
  Dim point As Point3d, point2 As Point3d
  Dim lngTemp As Long
  Dim oMessage As CadInputMessage

'  Send a keyin that can be a command string
  CadInputQueue.SendKeyin "SECTION CREATE "

'  Set a variable associated with a dialog box
  SetCExpressionValue "tcb->ms3DToolSettings.deleteBound.type", 0, "SECTION"

CadInputQueue.SendKeyin "SECTION CREATE "
SetCExpressionValue "tcb->ms3DToolSettings.deleteBound.type", 1, "SECTION"
CadInputQueue.SendKeyin "SECTION CREATE "
CadInputQueue.SendKeyin "DMSG ACTION KEYIN point keyin multiple"
CadInputQueue.SendKeyin "XY=0,0,590"
CadInputQueue.SendKeyin "XY=400000,0,590"
CadInputQueue.SendKeyin "XY=0,4000000,590"
CommandState.StartDefaultCommand

End Sub
```