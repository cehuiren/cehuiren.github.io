---
layout: post
title:  "MicroStation XM 断面处理程序源码-类-clsCopyElementCommand"
date: 2008-08-18
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 类
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>类：clsCopyElementCommand.cls

```vb
VERSION 1.0 CLASS
BEGIN
  MultiUse = -1  'True
END
Attribute VB_Name = "clsCopyElementCommand"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = False
Attribute VB_Exposed = False
'
'  Copyright: (c) 2004 Bentley Systems, Incorporated. All rights reserved.
'
Option Explicit

Implements ILocateCommandEvents

Private m_oStartElement As Element
Private m_tStartPoint As Point3d
Function CreateMovedElement(oBase As Element, oInputPoint As Point3d, addToFile As Boolean) As Element
    Dim oDistance As Point3d
    Dim oCopyContext As New CopyContext
    
    With oDistance
        .X = oInputPoint.X - m_tStartPoint.X
        .Y = oInputPoint.Y - m_tStartPoint.Y
        .Z = oInputPoint.Z - m_tStartPoint.Z
    End With
    
    oCopyContext.AddElementToModel = addToFile
    Set CreateMovedElement = ActiveModelReference.CopyElement(m_oStartElement, oCopyContext)
    CreateMovedElement.Move oDistance
End Function

'  MicroStation calls the Accept handler after the user
Private Sub ILocateCommandEvents_Accept(ByVal Element As Element, Point As Point3d, ByVal View As View)
    Dim oTemp As Element
    Set oTemp = CreateMovedElement(m_oStartElement, Point, True)

    oTemp.Redraw
    oTemp.Rewrite
    CommandState.StartLocate New clsCopyElementCommand
End Sub

Private Sub ILocateCommandEvents_Cleanup()

End Sub

Private Sub ILocateCommandEvents_Dynamics(Point As Point3d, ByVal View As View, ByVal DrawMode As MsdDrawingMode)
    Dim oTemp As Element
    
    m_oStartElement.Redraw
    Set oTemp = CreateMovedElement(m_oStartElement, Point, False)
    oTemp.Redraw DrawMode
End Sub

Private Sub ILocateCommandEvents_LocateFailed()
    CommandState.StartLocate New clsCopyElementCommand
End Sub

Private Sub ILocateCommandEvents_LocateFilter(ByVal Element As Element, Point As Point3d, Accepted As Boolean)
    ShowPrompt "Datapoint to define distance or Reset for another element"
    Set m_oStartElement = Element
    m_tStartPoint = Point
    CommandState.StartDynamics
End Sub

Private Sub ILocateCommandEvents_LocateReset()
    CommandState.StartLocate New clsCopyElementCommand
End Sub

Private Sub ILocateCommandEvents_Start()
    Dim lc As LocateCriteria
    
    '  Since this command does not modify the original element,
    '  set the locate criteria to allow the read-only elements.
    Set lc = CommandState.CreateLocateCriteria(False)
    CommandState.SetLocateCriteria lc
    
    ShowCommand "Copy Element"
    ShowPrompt "Select element to Copy"
End Sub
```