---
layout: post
title:  "MicroStation XM 断面处理程序源码-窗体-mapLevelFrm"
date: 2008-05-08
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>窗体：mapLevelFrm.frm

```vb
VERSION 5.00
Begin {C62A69F0-16DC-11CE-9E98-00AA00574A4F} mapLevelFrm 
   Caption         =   "绘等高线 (C)SurMap.com 覃东"
   ClientHeight    =   2520
   ClientLeft      =   45
   ClientTop       =   435
   ClientWidth     =   6240
   OleObjectBlob   =   "mapLevelFrm.frx":0000
   ShowModal       =   0   'False
   StartUpPosition =   1  '所有者中心
End
Attribute VB_Name = "mapLevelFrm"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = True
Attribute VB_Exposed = False

Private Sub cz1_Click()
If cz3.Value Then
SpinBtn.Enabled = True
CurvPara.Enabled = True
Else
SpinBtn.Enabled = False
CurvPara.Enabled = False
End If

End Sub

Private Sub cz2_Click()
If cz3.Value Then
SpinBtn.Enabled = True
CurvPara.Enabled = True
Else
SpinBtn.Enabled = False
CurvPara.Enabled = False
End If

End Sub

Private Sub cz3_Click()
If cz3.Value Then
SpinBtn.Enabled = True
CurvPara.Enabled = True
Else
SpinBtn.Enabled = False
CurvPara.Enabled = False
End If
End Sub

Private Sub DrawLevelLine_Click()
    Dim Point As Point3d, point2 As Point3d
    Dim lngTemp As Long
    Dim i As Long
    Dim LevelLine As Long '等高线高程值
    Dim curLevel As Level
    Dim dgxLevel As Level
    '取对象ID号判断是否生成了新的对象
    Dim LastEleId As Long
    Dim lvlId As Long
    Dim CvPara As Single
    Dim sEle As Element
    Dim cz As Integer '是否插值,1不插,2为中间插值,3为二次插值
    Dim sEles
    Dim sElestr As String
    Dim lvI As Long
    Dim Ele2D3D As String
'水面高程,等高线等深线颜色
Dim WaterLevel As Double
Dim MainLineColor As Integer
Dim CountLineColor As Integer
Dim WaterLineColor As Integer
CommandState.ErrorMessagesEnabled = False
CommandState.MessagesEnabled = False
If Trim(WaterLevelTxt.Text) <> "" And IsNumeric(Trim(WaterLevelTxt.Text)) Then
WaterLevel = Trim(WaterLevelTxt.Text)
Else
WaterLevel = 0
End If

If Trim(mlColor.Text) <> "" And IsNumeric(Trim(mlColor.Text)) Then
MainLineColor = Int(Trim(mlColor.Text))
If MainLineColor < 0 Or MainLineColor > 254 Then MainLineColor = 4
Else
MainLineColor = 4
End If


If Trim(ClColor.Text) <> "" And IsNumeric(Trim(ClColor.Text)) Then
CountLineColor = Int(Trim(ClColor.Text))
If CountLineColor < 0 Or CountLineColor > 254 Then CountLineColor = 4
Else
CountLineColor = 2
End If

If Trim(WlColor.Text) <> "" And IsNumeric(Trim(WlColor.Text)) Then
WaterLineColor = Int(Trim(WlColor.Text))
If WaterLineColor < 0 Or WaterLineColor > 254 Then WaterLineColor = 4
Else
WaterLineColor = 3
End If

If Ele2D3Dchk.Value Then
Ele2D3D = "2D"
Else
Ele2D3D = "3D"
End If

    
    If cz1.Value Then cz = 1
    If cz2.Value Then cz = 2
    If cz3.Value Then cz = 3
    
    LastEleId = ActiveModelReference.GraphicalElementCache.GetLastValidElement.CacheIndex

    CvPara = 0.1 + 0.03 * CurvPara
    
    If Trim(dgxDgj.Text) <> "" And IsNumeric(Trim(dgxDgj.Text)) Then
    dgxDist = Int(Trim(dgxDgj.Text))
    If dgxDist < 0 Then dgxDist = 5
    Else
    dgxDist = 5
    End If
    
        
    Set curLevel = ActiveSettings.Level
        LevelLine = Int(miniH / dgxDist) * dgxDist
        CadInputQueue.SendCommand "CONSTRUCT SECTION PLANE"
        'CadInputQueue.SendCommand "SECTION CREATE"
        
If sEleChk.Value = False Then
    For i = LevelLine To maxH Step dgxDist
    '   给当前命令传送一个数据点
        Point.X = miniE
        Point.Y = miniN
        Point.Z = i
        CadInputQueue.SendDataPoint Point, 1
    
        Point.X = maxE
        Point.Y = miniN
        Point.Z = i
        CadInputQueue.SendDataPoint Point, 1
    
        Point.X = maxE
        Point.Y = maxN
        Point.Z = i
        CadInputQueue.SendDataPoint Point, 1
    
        Point.X = miniE
        Point.Y = maxN
        Point.Z = i
        CadInputQueue.SendDataPoint Point, 1
        CadInputQueue.SendReset
    Next
Else
sElestr = Replace(Trim(sEleList.Text), " ", ",")
   ' If InStr(sElestr, ",") <> 0 Then
        sEles = Split(sElestr, ",")
        For lvI = 0 To UBound(sEles)
            If sEles(lvI) <> "" Then
                    Point.X = miniE
                    Point.Y = miniN
                    Point.Z = sEles(lvI)
                    CadInputQueue.SendDataPoint Point, 1
                
                    Point.X = maxE
                    Point.Y = miniN
                    Point.Z = sEles(lvI)
                    CadInputQueue.SendDataPoint Point, 1
                
                    Point.X = maxE
                    Point.Y = maxN
                    Point.Z = sEles(lvI)
                    CadInputQueue.SendDataPoint Point, 1
                
                    Point.X = miniE
                    Point.Y = maxN
                    Point.Z = sEles(lvI)
                    CadInputQueue.SendDataPoint Point, 1
                    CadInputQueue.SendReset
            End If
        Next
   ' End If
End If
ActiveSettings.Level = curLevel
SplineLevelLine_closed CvPara, cz, WaterLevel, MainLineColor, CountLineColor, WaterLineColor, Ele2D3D
SplineLevelLine_open CvPara, cz, WaterLevel, MainLineColor, CountLineColor, WaterLineColor, Ele2D3D
CommandState.MessagesEnabled = True
CommandState.ErrorMessagesEnabled = True
CommandState.StartDefaultCommand
End Sub
Private Sub dxCombo_Change()
Dim fw As String
Dim fw2
If Trim(dxCombo.Text) <> "" Then
  If ActiveModelReference.Name <> Trim(dxCombo.Text) Then
  If FindModel(Trim(dxCombo.Text)) Then ActiveDesignFile.Models(Trim(dxCombo.Text)).Activate
  End If
End If
fw = getModelXdata(ActiveModelReference, "surmap_fw", msdXDatumTypeString)
If Trim(fw) <> "" Then
fw2 = Split(fw, ",")
    If UBound(fw2) > 1 Then
    DrawLevelLine.Enabled = True
        miniN = fw2(0)
        miniE = fw2(1)
        miniH = fw2(2)
        
        maxN = fw2(3)
        maxE = fw2(4)
        maxH = fw2(5)
    End If
Else
        miniN = 0
        miniE = 0
        miniH = 0
        
        maxN = 0
        maxE = 0
        maxH = 0
    DrawLevelLine.Enabled = False
End If
End Sub
Private Sub sEleChk_Click()
If sEleChk.Value = True Then
sEleList.Enabled = True
Else
sEleList.Enabled = False
End If
End Sub

Private Sub SpinBtn_Change()
CurvPara = Format(SpinBtn.Value, "0")
End Sub

Private Sub UserForm_Initialize()
Dim fw As String
Dim fw2
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
        DrawLevelLine.Enabled = False
Else
        DrawLevelLine.Enabled = True
    If getModelXdata(ActiveModelReference, "surmap_dx", msdXDatumTypeString) = "dxmodel" Then
        dxCombo.Text = ActiveModelReference.Name
    Else
        dxCombo.ListIndex = 0
    End If
End If
fw = getModelXdata(ActiveModelReference, "surmap_fw", msdXDatumTypeString)
If Trim(fw) <> "" Then
fw2 = Split(fw, ",")
    If UBound(fw2) > 1 Then
    DrawLevelLine.Enabled = True
        miniN = fw2(0)
        miniE = fw2(1)
        miniH = fw2(2)
        
        maxN = fw2(3)
        maxE = fw2(4)
        maxH = fw2(5)
    End If
Else
        miniN = 0
        miniE = 0
        miniH = 0
        
        maxN = 0
        maxE = 0
        maxH = 0
    DrawLevelLine.Enabled = False
End If
End Sub

Private Sub UserForm_Layout()
SpinBtn.Value = CurvPara
End Sub
```