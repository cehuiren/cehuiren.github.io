---
layout: post
title:  "MicroStation XM 断面处理程序源码-窗体-loadPointsFrm"
date: 2008-05-09
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>窗体：loadPointsFrm.frm

```vb
VERSION 5.00
Begin {C62A69F0-16DC-11CE-9E98-00AA00574A4F} loadPointsFrm 
   Caption         =   "展点 (C)SurMap.com 覃东"
   ClientHeight    =   6360
   ClientLeft      =   45
   ClientTop       =   435
   ClientWidth     =   3810
   OleObjectBlob   =   "loadPointsFrm.frx":0000
   ShowModal       =   0   'False
   StartUpPosition =   1  '所有者中心
End
Attribute VB_Name = "loadPointsFrm"
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

Private Sub LoadPointsCom_Click()
Dim FileFmt As String
Dim zj As String
Dim pointXs As Integer
Dim pointColor As Integer
Dim remarkColor As Integer
Dim m As ModelReference
Dim adModelName As String
Dim cmScale As String

'提取绘图比例
cmScale = LoadScalePara(ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter")
If cmScale = "" Then
curScale = 500 / 1000
Else
curScale = cmScale / 1000
End If
'curScale = 500 / 1000



If Trim(dxCombo.Text) <> "" Then
    adModelName = Trim(dxCombo.Text)
    If Not FindModel(Trim(adModelName)) Then
            Set m = ActiveDesignFile.Models.Add(ActiveDesignFile.DefaultModelReference, Trim(adModelName), "地形模型(C)SurMap.com 覃东", msdModelTypeNormal, True)
               SetModelXdata m, "surmap_dx", msdXDatumTypeString, "dxmodel"
               m.Activate
    Else
               ActiveDesignFile.Models(adModelName).Activate
    End If
    
        If psneh.Value Then FileFmt = "psneh"
        If psenh.Value Then FileFmt = "psenh"
        If pneh.Value Then FileFmt = "pneh"
        If penh.Value Then FileFmt = "penh"
        If neh.Value Then FileFmt = "neh"
        If enh.Value Then FileFmt = "enh"
        If pntGc.Value Then zj = "gc"
        If pntDh.Value Then zj = "dh"
    If Trim(pntXs.Text) <> "" And IsNumeric(Trim(pntXs.Text)) And Trim(pntXs.Text) > 0 And Trim(pntXs.Text) < 5 Then
        pointXs = Trim(pntXs.Text)
    Else
        pointXs = 1
    End If
    If Trim(pColor.Text) <> "" And IsNumeric(Trim(pColor.Text)) And Trim(pColor.Text) >= 0 And Trim(pColor.Text) <= 254 Then
        pointColor = Trim(pColor.Text)
    Else
        pointColor = 0
    End If
    If Trim(rColor.Text) <> "" And IsNumeric(Trim(rColor.Text)) And Trim(rColor.Text) >= 0 And Trim(rColor.Text) <= 254 Then
        remarkColor = Trim(rColor.Text)
    Else
        remarkColor = 0
    End If
        If DataFileName.Text <> "" Then LoadPoints DataFileName.Text, FileFmt, zj, pointXs, pointColor, remarkColor
End If
 CadInputQueue.SendCommand "FIT VIEW EXTENDED"
End Sub

Private Sub OpenDataFile_Click()
Dim FileNum As Long
Dim DateLine As String
dataOpenDlg.DialogTitle = "打开地形数据文件 (C)SurMap.com 覃东"
dataOpenDlg.Filter = "地形数据文件(*.dat)|*.dat|文本文件(*.txt)|*.txt|逗号分隔文件(*.csv)|*.csv"
dataOpenDlg.ShowOpen
If dataOpenDlg.FileName <> "" Then
DataFileName = dataOpenDlg.FileName

    FileNum = FreeFile()
    Open dataOpenDlg.FileName For Input As FileNum
    If Not EOF(FileNum) Then
    Line Input #FileNum, DataLine 'pNum, pX, pY, pH
    sample = DataLine
    End If
    Close FileNum
Else
DataFileName = ""
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
    
    'BubbleSort ModelArray, j
    
    For i = 1 To j
'        ComboBox1.AddItem ActiveDesignFile.Models(i).Name
  '      If getModelXdata(ActiveDesignFile.Models(ModelArray(i)), "surmap_dx", msdXDatumTypeString) = "dxmodel" And ActiveDesignFile.Models(ModelArray(i)).Is3D Then dxCombo.AddItem ModelArray(i)
    Next

End Sub
```