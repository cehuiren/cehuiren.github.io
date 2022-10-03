---
layout: post
title:  "MicroStation XM 断面处理程序源码-模块-SubFuncForCell"
date: 2008-03-08
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>模块名：SubFuncForCell.bas

```vb
Attribute VB_Name = "SubFuncForCell"
Public CellLibAttached As Boolean

Sub AttachCellLib(CellLibName As String)
    AttachCellLibrary CellLibName, msdConversionModeAlways
    If IsCellLibraryAttached Then
    CellLibAttached = True
    Else
    CellLibAttached = False
    End If
End Sub

Sub AddPointCell(actModel As ModelReference, cellName As String, cellPoint As Point3d, cellColor As Integer, CellLevel As String, relatedPointID As DLong)
Dim adCell As CellElement
Dim adLinePoint1 As Point3d
Dim adLinePoint2 As Point3d
Dim adEle As TextElement
Dim adName As TextElement

Set adCell = CreateCellElement3(cellName, cellPoint, True)
actModel.AddElement adCell
adCell.Color = cellColor
Set adCell.Level = FindLevel(CellLevel)
SetElementXdata adCell, "surmap_controlpoint", msdXDatumTypeString, DLongToString(relatedPointID)
adCell.Rewrite
adCell.Redraw

adLinePoint1.X = cellPoint.X + 2
adLinePoint1.Y = cellPoint.Y
adLinePoint1.Z = 0
adLinePoint2.X = cellPoint.X + 5
adLinePoint2.Y = cellPoint.Y
adLinePoint2.Z = 0

CreateShortLineCell adLinePoint1, adLinePoint2, CellLevel, cellColor, 1, 0
DelPointLabel actModel, relatedPointID
End Sub

Function GetPointName(pId As DLong) As String
Dim LineEle As Element
Dim pntInfo As String
Dim pntInfos
           If ChkEleExist(pId, "surmap_points", "地形点标记") Then
            Set LineEle = ActiveModelReference.GetElementByID(peditLastPointID)
            pntInfo = getElementXdata(LineEle, "surmap_points", msdXDatumTypeString)
            pntInfos = Split(pntInfo, "|")
            GetPointName = pntInfos(0)
           Else
            GetPointName = ""
           End If
End Function

Sub CreateShortLineCell(spoint As Point3d, epoint As Point3d, LevelName As String, lColor As Integer, lStyle As Integer, lWeight As Integer)
Dim Lpoints(1) As Point3d
Dim cLine As LineElement

Lpoints(0) = spoint
Lpoints(1) = epoint

Set cLine = CreateLineElement1(Nothing, Lpoints)
cLine.Level = FindLevel(LevelName)
cLine.Color = lColor
cLine.LineStyle = ActiveDesignFile.LineStyles(lStyle)
cLine.LineWeight = lWeight
ActiveModelReference.AddElement cLine
cLine.Rewrite
cLine.Redraw
End Sub

Sub DelPointLabel(mdlIn As ModelReference, pId As DLong)
Dim labelId As DLong
Dim LabelEle As Element
Dim pntById As Element
Dim pntInfo As String
Dim pntInfos
            If ChkEleExist(pId, "surmap_points", "地形点标记") Then
                Set pntById = mdlIn.GetElementByID(pId)
                pntInfo = getElementXdata(pntById, "surmap_points", msdXDatumTypeString)
                pntInfos = Split(pntInfo, "|")
                labelId = DLongFromString(pntInfos(3))
                If ChkEleExist(labelId, "surmap_pointlabel", "地形点标注") Then
                Set LabelEle = mdlIn.GetElementByID(labelId)
                mdlIn.RemoveElement LabelEle
                End If
            End If
End Sub
```