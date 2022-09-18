---
layout: post
title:  "MicroStation XM 断面处理程序源码-类-clsSelectElement"
date: 2008-08-03
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 类
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>类：clsSelectElement.cls

```vb
VERSION 1.0 CLASS
BEGIN
  MultiUse = -1  'True
END
Attribute VB_Name = "clsSelectElement"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = False
Attribute VB_Exposed = True
Implements IPrimitiveCommandEvents

Private m_atPoints(0 To 1) As Point3d
Private m_nPoints As Integer


Private Sub IPrimitiveCommandEvents_Cleanup()

End Sub

'
'  Data point event handler
'
Private Sub IPrimitiveCommandEvents_DataPoint(Point As Point3d, ByVal View As View)
    Dim oEle As Element
    Dim lEle As LineElement
    ShowStatus ""
    On Error GoTo NoElement

    If m_nPoints = 0 Then
        CommandState.StartDynamics
        m_atPoints(0) = Point
        m_nPoints = 1
        ShowPrompt "Place end point"
    ElseIf m_nPoints = 1 Then
        m_atPoints(1) = Point
        m_nPoints = 0
        Set lEle = CreateLineElement1(Nothing, m_atPoints)
        ActiveModelReference.AddElement lEle
        lEle.Redraw
        LableLevelLine lEle
        CommandState.StartPrimitive Me
    End If
    
NoElement:

End Sub
Private Sub IPrimitiveCommandEvents_Dynamics(Point As Point3d, ByVal View As View, ByVal DrawMode As MsdDrawingMode)
    If m_nPoints = 1 Then
        m_atPoints(1) = Point
        Dim oEl As LineElement
        Set oEl = CreateLineElement1(Nothing, m_atPoints)
        oEl.Redraw DrawMode
    End If
End Sub

Private Sub IPrimitiveCommandEvents_Keyin(ByVal Keyin As String)

End Sub

Private Sub IPrimitiveCommandEvents_Reset()
    CommandState.StartDefaultCommand
    'CommandState.StartPrimitive Me
    'm_nPoints = 0
End Sub

'
'  Start event handler
'
Private Sub IPrimitiveCommandEvents_Start()
    ShowCommand "选择元素"
    ShowPrompt "请选择一条直线"
    'CommandState.SetLocateCursor
End Sub
```