---
layout: post
title:  "MicroStation XM 断面处理程序源码-类-clsEditPointSelect"
date: 2008-08-15
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 类
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>类：clsEditPointSelect.cls

```vb
VERSION 1.0 CLASS
BEGIN
  MultiUse = -1  'True
END
Attribute VB_Name = "clsEditPointSelect"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = False
Attribute VB_Exposed = False
Implements IPrimitiveCommandEvents

Private Sub IPrimitiveCommandEvents_Cleanup()

End Sub

'
'  Data point event handler
'
Private Sub IPrimitiveCommandEvents_DataPoint(Point As Point3d, ByVal View As View)
    Dim oEle As Element
    Dim LineEle As LineElement
    Dim xdataInfo As String
    Dim xdataInfos
    Dim pntInfo As String
    Dim pntInfos
    Dim pntId As DLong
    Dim pntIdstr As String
    On Error Resume Next
    ShowStatus ""
    On Error GoTo NoElement

    Set oEle = CommandState.LocateElement(Point, View, True)
    If Not (oEle Is Nothing) Then
        If oEle.HasXData("surmap_points") Then
            Set LineEle = oEle
            pntInfo = getElementXdata(LineEle, "surmap_points", msdXDatumTypeString)
            pntInfos = Split(pntInfo, "|")
            peditLastPointLabelID = DLongFromString(pntInfos(3))
            If ChkEleExist(peditLastPointLabelID, "surmap_pointlabel", "地形点标注") Then Set peditLastText = ActiveModelReference.GetElementByID(peditLastPointLabelID)
            Set peditLastPoint = oEle
            peditLastPointID = LineEle.id
            PointEditFrm.PntE = LineEle.StartPoint.X
            PointEditFrm.PntN = LineEle.StartPoint.Y
            PointEditFrm.PntH = LineEle.StartPoint.Z
            PointEditFrm.PntName = pntInfos(0)
            CommandState.StartPrimitive Me
        ElseIf oEle.HasXData("surmap_pointlabel") Then
           xdataInfo = getElementXdata(oEle, "surmap_pointlabel", msdXDatumTypeString)
           xdataInfos = Split(xdataInfo, "|")
           pntIdstr = xdataInfos(1)
           peditLastPointID = DLongFromString(pntIdstr)
           peditLastPointLabelID = oEle.id
           If ChkEleExist(peditLastPointID, "surmap_points", "地形点标记") Then
            Set LineEle = ActiveModelReference.GetElementByID(peditLastPointID)
           'End If
           ' If Not (lineEle Is Nothing) Then
            pntInfo = getElementXdata(LineEle, "surmap_points", msdXDatumTypeString)
            pntInfos = Split(pntInfo, "|")
            PointEditFrm.PntE = LineEle.StartPoint.X
            PointEditFrm.PntN = LineEle.StartPoint.Y
            PointEditFrm.PntH = LineEle.StartPoint.Z
            PointEditFrm.PntName = pntInfos(0)
            
            Set peditLastPoint = LineEle
            Set peditLastText = oEle
            PointEditFrm.LocatePoint.Enabled = True
            PointEditFrm.DelPointCom.Enabled = True
            PointEditFrm.ModifyCom.Enabled = True
            Else
            PointEditFrm.PntE = ""
            PointEditFrm.PntN = ""
            PointEditFrm.PntH = ""
            PointEditFrm.PntName = ""
            
            PointEditFrm.LocatePoint.Enabled = False
            PointEditFrm.DelPointCom.Enabled = False
            PointEditFrm.ModifyCom.Enabled = False
            End If
            CommandState.StartPrimitive Me
        Else
            ShowError "选择对象类型不正确!"
        End If
    Else
        CommandState.StartPrimitive Me
    End If
    Exit Sub
NoElement:
    ShowStatus "Element not found"

End Sub
Private Sub IPrimitiveCommandEvents_Dynamics(Point As Point3d, ByVal View As View, ByVal DrawMode As MsdDrawingMode)
End Sub

Private Sub IPrimitiveCommandEvents_Keyin(ByVal Keyin As String)

End Sub

Private Sub IPrimitiveCommandEvents_Reset()
    CommandState.StartPrimitive Me
End Sub

'
'  Start event handler
'
Private Sub IPrimitiveCommandEvents_Start()
    ShowCommand "选择地形点或标注"
    ShowPrompt "请选择地形点或标注"
    CommandState.SetLocateCursor
End Sub
```