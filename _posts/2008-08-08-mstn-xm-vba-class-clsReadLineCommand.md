---
layout: post
title:  "MicroStation XM 断面处理程序源码-类-clsReadLineCommand"
date: 2008-08-08
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 类
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>类：clsReadLineCommand.cls

```vb
VERSION 1.0 CLASS
BEGIN
  MultiUse = -1  'True
END
Attribute VB_Name = "clsReadLineCommand"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = False
Attribute VB_Exposed = False
Implements ILocateCommandEvents

Private Sub ILocateCommandEvents_Accept(ByVal Element As Element, Point As Point3d, ByVal View As View)
    Dim StartPnt As Point3d
    Dim EndPnt As Point3d
    Dim oLineElement As LineElement
    Dim LineVertices() As Point3d
    Dim i As Long

    Dim iDecimalPoint As Integer '小数位
    Dim iDelimiter As String '分隔符
    
    Dim iLevel As String
    Dim iColor As Integer
    Dim iStyle As String
    Dim iWeight As Integer
    
    Set oLineElement = Element
    
    iDelimiter = ","
    iDecimalPoint = 3

    LineInfoFrm.LineInfoTxt = ""
    LineVertices = oLineElement.GetVertices
    For i = 0 To UBound(LineVertices)
    LineInfoFrm.LineInfoTxt = LineInfoFrm.LineInfoTxt & Round(LineVertices(i).X, iDecimalPoint) & iDelimiter & Round(LineVertices(i).Y, iDecimalPoint) & iDelimiter & Round(LineVertices(i).Z, iDecimalPoint) & vbCrLf
    Next
   
    lpLineID = GetEleID(Element, "(C)SurMap.com ProfileLine")
    If Element.HasXData("(C)SurMap.com ProfileLine") And lpLineID <> "" Then
    LineInfoFrm.Li_All.Enabled = True
    Else
    LineInfoFrm.Li_All.Enabled = False
    End If
   
   
    lpSelected = True
    Set lpLineElement = Element
    iLevel = Element.Level.Name
    iColor = Element.Color
    iStyle = Element.LineStyle.Name
    iWeight = Element.LineWeight


    LineInfoFrm.Li_CLevel.Value = iLevel
    LineInfoFrm.Li_CColor.Value = iColor
    LineInfoFrm.Li_CStyle.Value = iStyle
    LineInfoFrm.Li_CWeight.Value = iWeight

    LineInfoFrm.Li_Levels.Value = iLevel
    LineInfoFrm.Li_SColor.Value = iColor
    LineInfoFrm.Li_SStyle.Value = iStyle
    LineInfoFrm.Li_SWeight.Value = iWeight
    CommandState.StartDefaultCommand
End Sub

Private Sub ILocateCommandEvents_Cleanup()

End Sub

Private Sub ILocateCommandEvents_Dynamics(Point As Point3d, ByVal View As View, _
        ByVal DrawMode As MsdDrawingMode)

End Sub

Private Sub ILocateCommandEvents_LocateFailed()
   ' ShowStatus "No arc element found"
End Sub

Private Sub ILocateCommandEvents_LocateFilter(ByVal Element As Element, Point As Point3d, Accepted As Boolean)
    '  Accepted defaults to False
    Accepted = False
    If Element.Type = msdElementTypeLine Or Element.Type = msdElementTypeLineString Then
        Accepted = True
    Else
        'lpSelected = False
        'Set lpLineElement = Nothing
        'LineInfoFrm.LineInfoTxt.Text = ""
        LineInfoFrm.LineInfoTxt = "您选择的元素不是直线元素!"
    End If

End Sub

Private Sub ILocateCommandEvents_LocateReset()
CommandState.StartDefaultCommand
End Sub

Private Sub ILocateCommandEvents_Start()
    Dim lc As LocateCriteria
    '  Since this command does not modify the original element,
    '  set the locate criteria to allow  read-only elements.
    Set lc = CommandState.CreateLocateCriteria(False)
    CommandState.SetLocateCriteria lc
    CommandState.EnableAccuSnap
    ShowCommand "设置断面线属性"
    ShowPrompt "请选择直线元素"
End Sub
```