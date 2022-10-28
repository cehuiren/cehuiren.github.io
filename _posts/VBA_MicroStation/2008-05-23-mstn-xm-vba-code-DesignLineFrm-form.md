---
layout: post
title:  "MicroStation XM 断面处理程序源码-窗体-DesignLineFrm"
date: 2008-05-23
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>窗体：DesignLineFrm.frm

```vb
VERSION 5.00
Begin {C62A69F0-16DC-11CE-9E98-00AA00574A4F} DesignLineFrm 
   Caption         =   "绘设计线程序 (C)SurMap.com 覃东"
   ClientHeight    =   7935
   ClientLeft      =   45
   ClientTop       =   435
   ClientWidth     =   6390
   OleObjectBlob   =   "DesignLineFrm.frx":0000
   ShowModal       =   0   'False
   StartUpPosition =   1  '所有者中心
End
Attribute VB_Name = "DesignLineFrm"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = True
Attribute VB_Exposed = False
Private Sub BatchDraw_Click()
Dim cmScale As String
Dim bComStrings
Dim i As Long

cmScale = LoadScalePara(ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter")
If cmScale = "" Then
curScale = 500 / 1000
Else
curScale = cmScale / 1000
End If
'curScale = 500 / 1000


bComStrings = Split(ComStringTxt.Text, vbCrLf)
comRows = 0
For i = 0 To UBound(bComStrings)
comRows = comRows + 1
If Trim(bComStrings(i)) <> "" Then doComm (bComStrings(i))
Next
End Sub

Private Sub ComStringTxt_Change()
If Trim(ComStringTxt) <> "" Then
DoCal.Enabled = True
Else
DoCal.Enabled = False
End If
End Sub

Private Sub ComStringTxt_KeyDown(ByVal KeyCode As MSForms.ReturnInteger, ByVal Shift As Integer)
If KeyCode = 13 Then KeyInChr = ""
End Sub

Private Sub ComStringTxt_KeyPress(ByVal KeyAscii As MSForms.ReturnInteger)
If KeyAscii <> 32 Then
KeyInChr = KeyInChr & Chr(KeyAscii)
Else
Select Case UCase(KeyInChr)
Case "I"
Echo = "语法：I 坡比(1/0.5，20/100),到达高程[,(L|R)]"
Case "A"
Echo = "语法：A 角度(D.MMSS),到达高程"
Case "H"
Echo = "语法：H 平距(正向右负向左)"
Case "V"
Echo = "语法：V 垂直距离(正向上负向下)"
Case "S"
Echo = "语法：S 坐标X,坐标Y"
Case "@"
Echo = "语法：@ 增量DX,增量DY"
Case "P"
Echo = "语法：P  坐标X,坐标Y"
Case "LEVEL"
Echo = "设定图层：LEVEL 图层名"
Case "COLOR"
Echo = "设定颜色：COLOR 颜色值(0～255)"
Case "STYLE"
Echo = "指定线性：STYLE 线性序号"
Case "WEIGHT"
Echo = "指定线宽：WEIGHT 线宽值(0～32)"
Case "WEIGHT"
Echo = "按函数表达式绘图:f(x) x^2+0.5*x,1,100,0.1"
Case Else
Echo = "命令不能被识别，输入可能有误！"
End Select

KeyInChr = ""
End If

End Sub

Private Sub comtxt_KeyDown(ByVal KeyCode As MSForms.ReturnInteger, ByVal Shift As Integer)
If KeyCode = 13 Then KeyInChr = ""
End Sub

Private Sub comtxt_KeyPress(ByVal KeyAscii As MSForms.ReturnInteger)
If KeyAscii <> 32 Then
KeyInChr = KeyInChr & Chr(KeyAscii)
Else
Select Case UCase(KeyInChr)
Case "I"
Echo = "语法：I 坡比(1/0.5，20/100),到达高程[,(L|R)]"
Case "A"
Echo = "语法：A 角度(D.MMSS),到达高程"
Case "H"
Echo = "语法：H 平距(正向右负向左)"
Case "V"
Echo = "语法：V 垂直距离(正向上负向下)"
Case "S"
Echo = "语法：S 坐标X,坐标Y"
Case "@"
Echo = "语法：@ 增量DX,增量DY"
Case "P"
Echo = "语法：P  坐标X,坐标Y"
Case "LEVEL"
Echo = "设定图层：LEVEL 图层名"
Case "COLOR"
Echo = "设定颜色：COLOR 颜色值(0～255)"
Case "STYLE"
Echo = "指定线性：STYLE 线性序号"
Case "WEIGHT"
Echo = "指定线宽：WEIGHT 线宽值(0～32)"
Case Else
Echo = "命令不能被识别，输入可能有误！"
End Select

KeyInChr = ""
End If
End Sub

Private Sub DoCal_Click()
'Dim cmScale As String
Dim bComStrings
Dim i As Long
Dim vpCoors() As String
Dim kwnCoor() As String
'cmScale = LoadScalePara(ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter")
'If cmScale = "" Then
'curScale = 500 / 1000
'Else
'curScale = cmScale / 1000
'End If
'curScale = 500 / 1000

vCoorList = ""
bComStrings = Split(ComStringTxt.Text, vbCrLf)
comRows = 0
For i = 0 To UBound(bComStrings)
comRows = comRows + 1
If Trim(bComStrings(i)) <> "" Then doCommForCal (bComStrings(i))
Next
vpCoors = Split(vCoorList, ",")
BubbleCoorListSortMath vpCoors, True
CalResult = ""
If Trim(toH) <> "" Then
CalResult = CalResult & "X-->Y    S-->H" & vbCrLf
CalResult = CalResult & "==============" & vbCrLf
CalResult = CalResult & "X" & vbTab & "Y" & vbCrLf
CalResult = CalResult & "--------------" & vbCrLf
kwnCoor = Split(Trim(toH), ",")
For i = LBound(kwnCoor) To UBound(kwnCoor)
If IsNumeric(kwnCoor(i)) Then
CalResult = CalResult & calH(vpCoors, Val(kwnCoor(i)))
End If
Next
End If
If CalResult <> "" Then CalResult = CalResult & vbCrLf
If Trim(toX) <> "" Then
CalResult = CalResult & "Y-->X     H--S" & vbCrLf
CalResult = CalResult & "==============" & vbCrLf
CalResult = CalResult & "Y" & vbTab & "X" & vbCrLf
CalResult = CalResult & "--------------" & vbCrLf
kwnCoor = Split(Trim(toX), ",")
For i = LBound(kwnCoor) To UBound(kwnCoor)
If IsNumeric(kwnCoor(i)) Then
'CalResult = CalResult & kwnCoor(i) & vbTab & Format(calX(vpCoors, Val(kwnCoor(i))), "0.000") & vbCrLf
CalResult = CalResult & calX(vpCoors, Val(kwnCoor(i)))
End If
Next
End If


End Sub

Private Sub DrawLine_Click()
doComm (comtxt.Text)
ComStringTxt.Text = ComStringTxt.Text & Trim(comtxt.Text) & vbCrLf
comtxt.Text = ""
End Sub




Private Sub hlpCom_Click()
HelpDesignLineFrm.Show
End Sub

Private Sub Load_Click()
Dim FileNum As Long
Dim comStr As String
ComStringTxt.Text = ""
CD.DialogTitle = "读取设计线定义文件"
CD.Filter = "设计线(*.sjx) (C)SurMap.com 覃东|*.sjx"
CD.ShowOpen
If CD.FileName <> "" Then
FileNum = FreeFile
Open CD.FileName For Input As FileNum

Do Until EOF(FileNum)
Line Input #FileNum, comStr
If Trim(comStr) <> "" Then ComStringTxt.Text = ComStringTxt.Text & comStr & vbCrLf
Loop


Close #FileNum
Echo = "已打开文件:" & CD.FileTitle
End If
End Sub

Private Sub ReadCom_Click()
    CommandState.StartPrimitive New clsSelectDesignLine
End Sub

Private Sub save_Click()
Dim FileNum As Long
CD.DialogTitle = "保存设计线定义文件"
CD.Filter = "设计线(*.sjx) (C)SurMap.com 覃东|*.sjx"
CD.ShowSave
If CD.FileName <> "" And Trim(ComStringTxt.Text) <> "" Then
FileNum = FreeFile
Open CD.FileName For Output As FileNum
Print #FileNum, UCase(Trim(ComStringTxt.Text))
Close #FileNum
Echo = "已保存为:" & CD.FileTitle
End If
End Sub

Private Sub UserForm_Initialize()
Echo = "版权所有 (C)2006-2008 SurMap.com 覃东"
preStr = "▽"
onPaperSize = 2 '标注在图尺上的打印尺寸为3毫米

curColor = 0
Set curLineStyle = ByLevelLineStyle
Set curLevel = FindLevel("缺省")
curWeight = 0
curPoint.X = 0
curPoint.Y = 0
curPoint.Z = 0
End Sub
```