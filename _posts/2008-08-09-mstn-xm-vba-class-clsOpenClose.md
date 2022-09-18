---
layout: post
title:  "MicroStation XM 断面处理程序源码-类-clsOpenClose"
date: 2008-08-09
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 类
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>类：clsOpenClose.cls

```vb
VERSION 1.0 CLASS
BEGIN
  MultiUse = -1  'True
END
Attribute VB_Name = "clsOpenClose"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = False
Attribute VB_Exposed = False
Dim WithEvents hooks As Application
Attribute hooks.VB_VarHelpID = -1
Private Sub Class_Initialize()
    Set hooks = Application
End Sub
Private Sub hooks_OnDesignFileClosed(ByVal DesignFileName As String)
    'ShowMessage "Closed design file " & DesignFileName
End Sub
Private Sub hooks_OnDesignFileOpened(ByVal DesignFileName As String)
    'splashForm.dgnFilename.Caption = DesignFileName
    Init
    'ToolsFrm.Show
    'ShowMessage "Opened design file " & DesignFileName
End Sub
```