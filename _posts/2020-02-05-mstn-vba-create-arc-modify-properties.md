---
layout: post
title:  "MicroStation VBA-创建圆弧或修改属性"
date: 2020-02-05
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA 加载 代码 圆弧 属性
excerpt: MicroStation VBA创建圆弧和修改元素属性示例代码。
author: 网格转载
---
* content
{:toc}

The following small code example describes how to

- create an arc,
- find all arcs in the active model and modify some properties like
  - center point
  - color
  - linestyle
  - line weight
- remove a specific element with given id from the active model.

```vb
Option Explicit  ' always recommended to use. Makes sure all variable are declared before use

Private elementID As DLong
Sub ArcCreate()

' Create ArcElement
Dim oArc As ArcElement
Dim p(2) As Point3d
    p(0) = Point3dZero
    p(1) = Point3dOne
    p(2) = Point3dFromXYZ(2, 0, 0)
    Set oArc = CreateArcElement3(Nothing, p(0), p(1), p(2))  ' This is one of 6 different functions to create an arc
    ActiveModelReference.AddElement oArc    ' after creation the elements needs to be added to the active model
    elementID = oArc.ID
End Sub

Sub ArcSearchAndModify()
' Scan active model for arcs and change some properties
Dim oArc As ArcElement
Dim ee As ElementEnumerator
Dim sc As New ElementScanCriteria   ' Scan criteria to search for specific attributes
    sc.ExcludeAllTypes
    sc.IncludeType msdElementTypeArc    ' search for arcs
    Set ee = ActiveModelReference.Scan(sc)  ' scan active model for all elements matching the scan citeria
    Do While ee.MoveNext
        Set oArc = ee.Current.AsArcElement
        Dim pCenter As Point3d
        pCenter = oArc.CenterPoint
        Dim pMove As Point3d
        pMove = Point3dFromXYZ(1, -1, -1)
        oArc.Move pMove
        oArc.PrimaryRadius = oArc.PrimaryRadius * 1.2
        oArc.Color = 11
        oArc.LineWeight = 4
        Set oArc.LineStyle = ActiveDesignFile.LineStyles(5)
        oArc.Rewrite ' important to rewrite element in the designfile to save changes
    Loop
End Sub

Sub ArcDelete()
    Dim oArc As ArcElement
    'on error resume next
    If DLongToString(elementID) <> "0" Then
        Set oArc = ActiveModelReference.GetElementByID(elementID) ' get Arc with given ID
        Debug.Print DLongToString(oArc.ID), CStr(oArc.ID64)
        ActiveModelReference.RemoveElement oArc ' delete element from active model
        elementID = DLongFromString("0")
    End If
End Sub
```