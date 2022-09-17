---
layout: post
title:  "MicroStation XM 断面处理程序源码-窗体-DtmEditFrm"
date: 2008-05-15
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>窗体：DtmEditFrm.frm

```vb
VERSION 5.00
Begin {C62A69F0-16DC-11CE-9E98-00AA00574A4F} DtmEditFrm 
   Caption         =   "三角网(DTM)编辑 (C)SurMap.com 覃东"
   ClientHeight    =   1095
   ClientLeft      =   45
   ClientTop       =   435
   ClientWidth     =   5895
   OleObjectBlob   =   "DtmEditFrm.frx":0000
   ShowModal       =   0   'False
   StartUpPosition =   1  '所有者中心
End
Attribute VB_Name = "DtmEditFrm"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = True
Attribute VB_Exposed = False

Private Sub AddTri_Click()
    CadInputQueue.SendCommand "PLACE SHAPE CONSTRAINED"
End Sub

Private Sub DelTri_Click()
    CommandState.StartLocate New clsDelTriangles
End Sub

Private Sub DropDtm_Click()
    Dim triEe As ElementEnumerator
    Dim oScanCriteria As ElementScanCriteria
    Dim TriShp As ShapeElement
    Dim Point As Point3d
    Dim triGrp As NamedGroupElement
    Dim LastId As Long
    Dim pBar As Long
    pBar = 0
'PPbar.Visible = True
Label1.Visible = False
dxCombo.Visible = False
DtmEditFrm.Repaint
ShowMessage "开始准备工作请等待..."
    LastId = ActiveModelReference.GraphicalElementCache.GetLastValidElement.CacheIndex
    If ActiveModelReference.GetNamedGroup("triangles") Is Nothing Then
    Set triGrp = ActiveModelReference.AddNewNamedGroup("triangles")
    Else
    Set triGrp = ActiveModelReference.GetNamedGroup("triangles")
    End If

    Set oScanCriteria = New ElementScanCriteria
    oScanCriteria.ExcludeAllTypes
    oScanCriteria.IncludeType msdElementTypeMeshHeader

    Set triEe = ActiveModelReference.GraphicalElementCache.Scan(oScanCriteria)

     Do While triEe.MoveNext
       If triEe.Current.Type = msdElementTypeMeshHeader Then
            ActiveModelReference.SelectElement triEe.Current
       End If
    Loop
    CadInputQueue.SendCommand "DROP ELEMENT"
    Point.X = 0
    Point.Y = 0
    Point.Z = 0
    CadInputQueue.SendDataPoint Point, 1
    
    
    If LastId <> ActiveModelReference.GraphicalElementCache.GetLastValidElement.CacheIndex Then
    Set oScanCriteria = New ElementScanCriteria
    oScanCriteria.ExcludeAllTypes
    oScanCriteria.IncludeType msdElementTypeShape
    oScanCriteria.ExcludeAllLevels
    oScanCriteria.IncludeLevel FindLevel("三角网")
    Set triEe = ActiveModelReference.GraphicalElementCache.Scan(oScanCriteria)
     Do While triEe.MoveNext
            If pBar < 1000 Then
            pBar = pBar + 1
            Else
            pBar = 1
            End If
'            PPbar.Value = pBar
            Set TriShp = triEe.Current
            If TriShp.VerticesCount = 4 And triGrp.GetMember(triEe.Current) Is Nothing Then triGrp.AddMember triEe.Current
     Loop
     triGrp.Rewrite
     End If
     
     LastElementID = ActiveModelReference.GraphicalElementCache.GetLastValidElement.CacheIndex
ShowMessage "准备工作结束!"
'PPbar.Visible = False
Label1.Visible = True
dxCombo.Visible = True
End Sub

Private Sub dxCombo_Change()
If Trim(dxCombo.Text) <> "" Then
  If ActiveModelReference.Name <> Trim(dxCombo.Text) Then
  If FindModel(Trim(dxCombo.Text)) Then ActiveDesignFile.Models(Trim(dxCombo.Text)).Activate
  End If
End If
End Sub

Private Sub TriChange_Click()
Dim ShpVer1() As Point3d
Dim ShpVer2() As Point3d
Dim SamePoints As Integer
Dim Ee As ElementEnumerator
Dim Shps(100) As ShapeElement
Dim Shp As ShapeElement
Dim Cnt As Long
SamePoints = 0
Cnt = 0
     Set Ee = ActiveModelReference.GetSelectedElements
     Do While Ee.MoveNext
       If Ee.Current.Type = msdElementTypeShape Then
            Set Shp = Ee.Current
            If Shp.VerticesCount = 4 And Cnt < 100 Then
            Set Shps(Cnt) = Ee.Current
            Cnt = Cnt + 1
            End If
       End If
    Loop
        
    If Cnt = 2 Then
        ShpVer1 = Shps(0).GetVertices
        ShpVer2 = Shps(1).GetVertices
        For i = 0 To UBound(ShpVer1) - 1
          For j = 0 To UBound(ShpVer2) - 1
          If (ShpVer2(j).X = ShpVer1(i).X) And (ShpVer2(j).Y = ShpVer1(i).Y) And (ShpVer2(j).Z = ShpVer1(i).Z) Then
          SamePoints = SamePoints + 1
          End If
          Next
        Next
     If SamePoints = 2 Then ChangeShape Shps(0), Shps(1)
   End If
End Sub

Private Sub UpDate_Click()
    Dim triEe As ElementEnumerator
    Dim oScanCriteria As ElementScanCriteria
    Dim TriShp As ShapeElement
    Dim Point As Point3d
    Dim LastId As Long
    Dim CurId As Long
    Dim LastEle As Element
    Dim triGrp As NamedGroupElement
    Dim cEleID As Long
    Dim i As Long
    Dim LastElementTmp As Element
    
    'Set dtmGrp = ActiveModelReference.GetNamedGroup("dtm")
    ActiveModelReference.UnselectAllElements
    CadInputQueue.SendCommand "DIALOG TOOLBOX MESH TOGGLE"
    CadInputQueue.SendCommand "FACET MODIFY stitch"
    '   设置一个与对话框关联的变量
    SetCExpressionValue "tcb->ms3DToolSettings.trimCurve.meshModifyType", 1, "FACET"

    LastId = ActiveModelReference.GraphicalElementCache.GetLastValidElement.CacheIndex
    
    
    'dtmGrp.SelectElements msdMemberTraverseSimple
If ActiveModelReference.GetNamedGroup("triangles") Is Nothing Or ActiveModelReference.GetNamedGroup("triangles").MembersCount = 0 Then
    Set oScanCriteria = New ElementScanCriteria
    oScanCriteria.ExcludeAllTypes
    oScanCriteria.IncludeType msdElementTypeShape

        Set triEe = ActiveModelReference.GraphicalElementCache.Scan(oScanCriteria)

     Do While triEe.MoveNext
       If triEe.Current.Type = msdElementTypeShape And triEe.Current.IsValid Then
            Set TriShp = triEe.Current
            If TriShp.VerticesCount = 4 Then ActiveModelReference.SelectElement triEe.Current
       End If
    Loop
Else
    'LastElementID = LastId
    Set triGrp = ActiveModelReference.GetNamedGroup("triangles")
    If LastElementID <> -1 And LastId > LastElementID Then
    For i = LastElementID + 1 To LastId
    If ActiveModelReference.GraphicalElementCache.IsElementValid(i) Then
    Set LastElementTmp = ActiveModelReference.GraphicalElementCache.GetElement(i)
    If LastElementTmp.Type = msdElementTypeShape And triGrp.GetMember(LastElementTmp) Is Nothing Then
    LastElementTmp.Color = 7
    LastElementTmp.Rewrite
    triGrp.AddMember LastElementTmp
    End If
    End If
    Next
    End If
    triGrp.SelectElements msdMemberTraverseSimple
End If
    CadInputQueue.SendCommand "FACET MODIFY stitch"
    Point.X = 0
    Point.Y = 0
    Point.Z = 0
    CadInputQueue.SendDataPoint Point, 1
    CurId = ActiveModelReference.GraphicalElementCache.GetLastValidElement.CacheIndex
    If LastId <> CurId Then
    Set LastEle = ActiveModelReference.GraphicalElementCache.GetLastValidElement
    LastEle.Level = FindLevel("三角网")
    LastEle.Color = 9
    LastEle.Rewrite
    LastEle.Redraw
    End If

End Sub

Private Sub UserForm_Initialize()
    Dim i As Long
    Dim j As Long
    Dim ModelArray() As String
    j = ActiveDesignFile.Models.Count
    ReDim ModelArray(j)
    
    For i = 1 To j
        ModelArray(i) = ActiveDesignFile.Models(i).Name
    Next
    
    BubbleSort ModelArray, j
    
    For i = 1 To j
        If getModelXdata(ActiveDesignFile.Models(ModelArray(i)), "surmap_dx", msdXDatumTypeString) = "dxmodel" And ActiveDesignFile.Models(ModelArray(i)).Is3D Then dxCombo.AddItem ModelArray(i)
    Next
 

If dxCombo.ListCount = 0 Then
        'genTriCom.Enabled = False
Else
        'genTriCom.Enabled = True
    If getModelXdata(ActiveModelReference, "surmap_dx", msdXDatumTypeString) = "dxmodel" Then
        dxCombo.Text = ActiveModelReference.Name
    Else
        dxCombo.ListIndex = 0
    End If
End If
LastElementID = -1
End Sub
```