---
title: MicroStation VBA接口-IPrimitiveCommandEvents示例
author: QinDong
date: 2022-11-06 16:40:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 接口 代码
excerpt: 
---
* content
{:toc}

### 类：PCE_LineTest
```vb
Implements IPrimitiveCommandEvents
Dim pt3BasePoint As Point3d
Dim boolSet As Boolean

Private Sub IPrimitiveCommandEvents_Cleanup()

End Sub

Private Sub IPrimitiveCommandEvents_DataPoint(Point As Point3d, ByVal View As View)
    If boolSet = False Then
        pt3BasePoint = Point
        CommandState.StartDynamics
        boolSet = True
    Else
        CommandState.StartDefaultCommand
    End If
End Sub

Private Sub IPrimitiveCommandEvents_Dynamics(Point As Point3d, ByVal View As View, ByVal DrawMode As MsdDrawingMode)
    Dim myLineElem As LineElement
    Set myLineElem = CreateLineElement2(Nothing, pt3BasePoint, Point)
    myLineElem.Redraw DrawMode
End Sub

Private Sub IPrimitiveCommandEvents_Keyin(ByVal Keyin As String)

End Sub

Private Sub IPrimitiveCommandEvents_Reset()

End Sub

Private Sub IPrimitiveCommandEvents_Start()

End Sub
```
{: .nolineno}

### 模块：StartPCE
```vb
Sub PlaceLine()
    CommandState.StartPrimitive New PCE_LineTest
End Sub
```

### 例二
```vb
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
{: .nolineno }