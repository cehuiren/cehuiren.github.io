---
layout: post
title:  "MicroStation XM 断面处理程序源码-窗体-TrianglesFrm"
date: 2008-04-01
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>窗体：TrianglesFrm.frm

```vb
VERSION 5.00
Begin {C62A69F0-16DC-11CE-9E98-00AA00574A4F} TrianglesFrm 
   Caption         =   "生成三角网 (C)SurMap.com 覃东"
   ClientHeight    =   1710
   ClientLeft      =   45
   ClientTop       =   435
   ClientWidth     =   6900
   OleObjectBlob   =   "TrianglesFrm.frx":0000
   ShowModal       =   0   'False
   StartUpPosition =   1  '所有者中心
End
Attribute VB_Name = "TrianglesFrm"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = True
Attribute VB_Exposed = False

Private Sub dxCombo_Change()
If Trim(dxCombo.Text) <> "" Then
  If ActiveModelReference.Name <> Trim(dxCombo.Text) Then
  If FindModel(Trim(dxCombo.Text)) Then ActiveDesignFile.Models(Trim(dxCombo.Text)).Activate
  End If
End If

'  If ActiveModelReference.Name <> Trim(dxCombo.Text) Then
'  If FindModel(Trim(dxCombo.Text)) And Trim(dxCombo.Text) <> "" Then ActiveDesignFile.Models(Trim(dxCombo.Text)).Activate
'  End If
End Sub

Private Sub genTriCom_Click()
If AutoSel.Value = True Then DrawTriangles 1
If ManSel.Value = True Then DrawTriangles 2
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
        genTriCom.Enabled = False
Else
        genTriCom.Enabled = True
    If getModelXdata(ActiveModelReference, "surmap_dx", msdXDatumTypeString) = "dxmodel" Then
        dxCombo.Text = ActiveModelReference.Name
    Else
        dxCombo.ListIndex = 0
    End If
End If
End Sub
```