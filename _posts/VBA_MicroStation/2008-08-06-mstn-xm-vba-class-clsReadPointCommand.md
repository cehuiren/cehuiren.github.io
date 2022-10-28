---
layout: post
title:  "MicroStation XM 断面处理程序源码-类-clsReadPointCommand"
date: 2008-08-06
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 类
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>类：clsReadPointCommand.cls

```vb
VERSION 1.0 CLASS
BEGIN
  MultiUse = -1  'True
END
Attribute VB_Name = "clsReadPointCommand"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = False
Attribute VB_Exposed = False
Implements ILocateCommandEvents

Private Sub ILocateCommandEvents_Accept(ByVal Element As Element, Point As Point3d, ByVal View As View)

End Sub

Private Sub ILocateCommandEvents_Cleanup()

End Sub

Private Sub ILocateCommandEvents_Dynamics(Point As Point3d, ByVal View As View, ByVal DrawMode As MsdDrawingMode)

End Sub

Private Sub ILocateCommandEvents_LocateFailed()
    ShowStatus "请选择元素上的点！"
End Sub

Private Sub ILocateCommandEvents_LocateFilter(ByVal Element As Element, Point As Point3d, Accepted As Boolean)
DspMsg.X.Text = Round(Point.X, 3)
DspMsg.Y.Text = Round(Point.Y, 3)
DspMsg.Z.Text = Round(Point.Z, 3)
DspMsg.Show
End Sub

Private Sub ILocateCommandEvents_LocateReset()

End Sub

Private Sub ILocateCommandEvents_Start()
    Dim lc As LocateCriteria
    
    '  Since this command does not modify the original element,
    '  set the locate criteria to allow  read-only elements.
    Set lc = CommandState.CreateLocateCriteria(False)
    CommandState.SetLocateCriteria lc
    CommandState.EnableAccuSnap
    ShowCommand "读取点坐标"
    ShowPrompt "请选择一个点,可以捕捉:"
End Sub
```