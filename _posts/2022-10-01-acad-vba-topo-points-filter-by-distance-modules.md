---
title: AutoCAD VBA地形点抽稀-模块
author: QinDong
date: 2022-10-02 08:27:00 +0800
categories: 编程开发 VBA-AutoCAD
tags: 地形 代码
excerpt: 
---
* content
{:toc}

```vb
Public Sub vba_zzDcx()
frm_CassDatFilter.show
End Sub


Public Sub AcadStartUp()
Call CreateToolbarExample
End Sub


Public Sub CreateToolbarExample()
Dim mnuGroup As AcadMenuGroup
Dim tbTest As AcadToolbar
Dim tbCopy As AcadToolbarItem
Dim tbPaste As AcadToolbarItem
Dim tbSeparator As AcadToolbarItem
Dim macCopy As String
Dim macPasteclip As String
Dim strPath1 As String
Dim strPath2 As String
Set mnuGroup = ThisDrawing.Application.MenuGroups.Item(0)
Set tbTest = mnuGroup.Toolbars.Add("抽稀")
macCopy = Chr(3) & Chr(3) & Chr(95) & "copy" & Chr(32)
macPaste = Chr(3) & Chr(3) & Chr(95) & "pasteclip" & Chr(32)
Set tbCopy = tbTest.AddToolbarButton _
(tbTest.Count + 1, "复制", "复制", macCopy, False)
Set tbPaste = tbTest.AddToolbarButton _
(tbTest.Count + 1, "粘贴 ", "粘贴", macPaste, False)
Set tbSeparator = tbTest.AddSeparator(tbTest.Count + 1)
'????????
'strPath1 = "G:\VBA\copy.bmp"
'strPath2 = "G:\VBA\copy.bmp"
'tbCopy.SetBitmaps strPath1, strPath2
'strPath1 = "G:\VBA\paste.bmp"
'strPath2 = "G:\VBA\paste.bmp"
'tbPaste.SetBitmaps strPath1, strPath2
'MsgBox "左"
tbTest.Dock acToolbarDockLeft
'MsgBox "右"
'tbTest.Float 550, 300, 1
End Sub
```
{: file='Modules-CassDatFilter'}