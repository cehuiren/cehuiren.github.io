---
layout: post
title:  "MicroStation XM 断面处理程序源码-类-clsSelectDesignLine"
date: 2008-08-05
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 类
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>类：clsSelectDesignLine.cls

```vb
VERSION 1.0 CLASS
BEGIN
  MultiUse = -1  'True
END
Attribute VB_Name = "clsSelectDesignLine"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = False
Attribute VB_Exposed = False
Implements IPrimitiveCommandEvents



Private Sub IPrimitiveCommandEvents_Cleanup()

End Sub

'
'  Data point event handler
'
Private Sub IPrimitiveCommandEvents_DataPoint(Point As Point3d, ByVal View As View)
    Dim oEle As Element
    Dim lEle As LineElement
    Dim cmsEle As ComplexStringElement
    Dim LineVers() As Point3d
    Dim dsnI As Long
    ShowStatus ""
    On Error GoTo NoElement
    Set oEle = CommandState.LocateElement(Point, View, True)
   ' MsgBox oEle.Type
    If Not (oEle Is Nothing) Then
        If oEle.Type = msdElementTypeLine Or oEle.Type = msdElementTypeLineString Then
            Set lEle = oEle
            LineVers = lEle.GetVertices
            DesignLineFrm.ComStringTxt.Text = "S " & lEle.StartPoint.X & "," & lEle.StartPoint.Y
            For dsnI = 1 To UBound(LineVers)
            DesignLineFrm.ComStringTxt.Text = DesignLineFrm.ComStringTxt.Text & vbCrLf & "P " & LineVers(dsnI).X & "," & LineVers(dsnI).Y
            Next
            CommandState.StartPrimitive Me
        ElseIf oEle.Type = msdElementTypeComplexString Then
            Set cmsEle = oEle
            'LineVers = cmsEle.get
            DesignLineFrm.ComStringTxt.Text = "S " & lEle.StartPoint.X & "," & lEle.StartPoint.Y
            For dsnI = 1 To UBound(LineVers)
            DesignLineFrm.ComStringTxt.Text = DesignLineFrm.ComStringTxt.Text & vbCrLf & "P " & LineVers(dsnI).X & "," & LineVers(dsnI).Y
            Next
            CommandState.StartPrimitive Me
        Else
            MsgBox oEle.Type
            ShowError "请选择一条件直线元素!"
        End If
    Else
        CommandState.StartPrimitive Me
    End If
    Exit Sub
NoElement:
    ShowStatus "Element not found"
'    Dim oEle As Element
'    ShowStatus ""
'    On Error GoTo NoElement
'
'    Set oEle = CommandState.LocateElement(Point, View, True)
'    If Not (oEle Is Nothing) Then
'        MsgBox oEle.Type
'        If oEle.Type = msdElementTypeLine Then
'            MsgBox "OK:"
'            CommandState.StartPrimitive Me
'        Else
'            ShowError "That element cannot be dropped"
'        End If
'    Else
'        CommandState.StartPrimitive Me
'    End If
'    Exit Sub
'NoElement:
'    ShowStatus "Element not found"



End Sub
Private Sub IPrimitiveCommandEvents_Dynamics(Point As Point3d, ByVal View As View, ByVal DrawMode As MsdDrawingMode)
End Sub

Private Sub IPrimitiveCommandEvents_Keyin(ByVal Keyin As String)

End Sub

Private Sub IPrimitiveCommandEvents_Reset()
    CommandState.StartDefaultCommand
    'CommandState.StartPrimitive Me
    'm_nPoints = 0
End Sub

'
'  Start event handler
'
Private Sub IPrimitiveCommandEvents_Start()
    ShowCommand "选择元素"
    ShowPrompt "请选择一条直线"
    CommandState.SetLocateCursor
End Sub
```