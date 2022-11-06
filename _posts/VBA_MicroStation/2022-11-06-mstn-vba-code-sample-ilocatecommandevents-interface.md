---
title: MicroStation VBA接口-ILocateCommandEvents示例
author: QinDong
date: 2022-11-06 16:50:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 接口 代码
excerpt: 
---
* content
{:toc}

### 类clsReadLineCommand
```vb
Implements ILocateCommandEvents

Private Sub ILocateCommandEvents_Accept(ByVal Element As Element, Point As Point3d, ByVal View As View)
    Dim StartPnt As Point3d
    Dim EndPnt As Point3d
    Dim oLineElement As LineElement
    Dim LineVertices() As Point3d
    Dim I As Long

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
    For I = 0 To UBound(LineVertices)
    LineInfoFrm.LineInfoTxt = LineInfoFrm.LineInfoTxt & Round(LineVertices(I).X, iDecimalPoint) & iDelimiter & Round(LineVertices(I).Y, iDecimalPoint) & iDelimiter & Round(LineVertices(I).Z, iDecimalPoint) & vbCrLf
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
{: .nolineno}

### 模块(任一过程内)
```vb
Private Sub Li_Select_Click()
    lpSelected = False
    CommandState.StartLocate New clsReadLineCommand
End Sub
```
{: .nolineno}