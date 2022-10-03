---
layout: post
title:  "MicroStation XM 断面处理程序源码-类-clsDelTriangles"
date: 2008-08-17
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 类
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>类：clsDelTriangles.cls

```vb
VERSION 1.0 CLASS
BEGIN
  MultiUse = -1  'True
END
Attribute VB_Name = "clsDelTriangles"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = False
Attribute VB_Exposed = False
Implements ILocateCommandEvents

Private Sub ILocateCommandEvents_Accept(ByVal Element As Element, Point As Point3d, ByVal View As View)
'Dim oDE As m
'Dim oEE As ElementEnumerator
'Dim oNew As Element
'
'Set oDE = Element
ActiveModelReference.RemoveElement Element
'If Element.IsDroppableElement Then MsgBox "fasdf"
'If Element.IsDroppableElement Then
'Set oEE = oDE.Drop
'
'        ActiveModelReference.RemoveElement Element
'        Do While oEE.MoveNext
'            Set oNew = oEE.Current
'            ActiveModelReference.AddElement oNew
'            oNew.Redraw msdDrawingModeNormal
'        Loop
'
''End If
'    CommandState.StartDefaultCommand
End Sub

Private Sub ILocateCommandEvents_Cleanup()

End Sub

Private Sub ILocateCommandEvents_Dynamics(Point As Point3d, ByVal View As View, ByVal DrawMode As MsdDrawingMode)

End Sub

Private Sub ILocateCommandEvents_LocateFailed()
   ' ShowStatus "No arc element found"
End Sub

Private Sub ILocateCommandEvents_LocateFilter(ByVal Element As Element, Point As Point3d, Accepted As Boolean)
    '  Accepted defaults to False
'If Element.IsDroppableElement Then

'    Accepted = False
    
'    If Element.Type = 105 Then
        Accepted = True
'    Else
        'lpSelected = False
        'Set lpLineElement = Nothing
        'LineInfoFrm.LineInfoTxt.Text = ""
       ' LineInfoFrm.LineInfoTxt = "您选择的元素不是直线元素!"
 '   End If

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
   ' CommandState.EnableAccuSnap
    ShowCommand "设置断面线属性"
    ShowPrompt "请选择直线元素"
End Sub
```