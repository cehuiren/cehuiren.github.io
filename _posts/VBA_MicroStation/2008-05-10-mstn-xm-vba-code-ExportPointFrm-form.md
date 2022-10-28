---
layout: post
title:  "MicroStation XM 断面处理程序源码-窗体-ExportPointFrm"
date: 2008-05-10
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>窗体：ExportPointFrm.frm

```vb
VERSION 5.00
Begin {C62A69F0-16DC-11CE-9E98-00AA00574A4F} ExportPointFrm 
   Caption         =   "地形点导出 (C)SurMap.com 覃东"
   ClientHeight    =   4170
   ClientLeft      =   45
   ClientTop       =   435
   ClientWidth     =   6240
   OleObjectBlob   =   "ExportPointFrm.frx":0000
   ShowModal       =   0   'False
   StartUpPosition =   1  '所有者中心
End
Attribute VB_Name = "ExportPointFrm"
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
End Sub

Private Sub exOpenFileCom_Click()
expCD.DialogTitle = "导出地形点为数据文件 (C)SurMap.com 覃东"
expCD.Filter = "地形数据文件(*.dat)|*.dat|文本文件(*.txt)|*.txt|逗号分隔文件(*.csv)|*.csv"
expCD.ShowSave
If Trim(expCD.FileName) <> "" Then
expFileName.Text = expCD.FileName
End If
End Sub

Private Sub ExpCom_Click()
Dim FileFmt As String
Dim SelMethod As Integer
        If psneh.Value Then FileFmt = "psneh"
        If psenh.Value Then FileFmt = "psenh"
        If pneh.Value Then FileFmt = "pneh"
        If penh.Value Then FileFmt = "penh"
        If neh.Value Then FileFmt = "neh"
        If enh.Value Then FileFmt = "enh"
        If AutoSel.Value Then SelMethod = 1
        If ManSel.Value Then SelMethod = 2
If Trim(dxCombo.Text) <> "" And Trim(expFileName.Text) <> "" Then ExportPoints Trim(expFileName.Text), FileFmt, SelMethod
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
        ExpCom.Enabled = False
Else
        ExpCom.Enabled = True
    If getModelXdata(ActiveModelReference, "surmap_dx", msdXDatumTypeString) = "dxmodel" Then
        dxCombo.Text = ActiveModelReference.Name
    Else
        dxCombo.ListIndex = 0
    End If
End If

End Sub
```