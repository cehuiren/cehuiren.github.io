---
layout: post
title:  "MicroStation XM 断面处理程序源码-模块-menu"
date: 2008-03-06
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>模块名：menu.bas

```vb
Attribute VB_Name = "menu"
Sub LoadPointsCom()
loadPointsFrm.Show
End Sub

Sub ExportPointsCom()
ExportPointFrm.Show
End Sub

Sub MakeDTMCom()
TrianglesFrm.Show
End Sub

Sub DTMEditCom()
If RegOk Then
DtmEditFrm.Show
Else
RegFrm.Show
End If
End Sub

Sub MakeProfileCom()
If RegOk Then
mapProfileSet.Show
Else
RegFrm.Show
End If
SectionInit
End Sub

Sub MakeLevelCom()
mapLevelFrm.Show
SectionInit
End Sub

Sub LabelLevelLineCom()
    CommandState.StartPrimitive New clsSelectElement
End Sub

Sub delLevelLineCom()
DelLevelLine
End Sub

Sub ProfilesListCom()
ViewModelFrm.Show
End Sub

Sub DrawProfilesCom()
LineFileFrm.Show
End Sub

Sub ProfileLineInfoCom()
LineInfoFrm.Show
End Sub

Sub DesignLinesCom()
If RegOk Then
DesignLineFrm.Show
Else
RegFrm.Show
End If
End Sub

Sub SetOriginCom()
soFrm.Show
End Sub

Sub SetParaCom()
SetParaFrm.Show
End Sub

Sub OnlineHelpCom()
Shell "explorer http://www.surmap.com/zt/ustationmap/"
End Sub

Sub AboutMeCom()
splashForm.Show
End Sub

Sub RegistorCom()
RegFrm.Show
End Sub

Sub PointEditCom()
PointEditFrm.Show
End Sub
```