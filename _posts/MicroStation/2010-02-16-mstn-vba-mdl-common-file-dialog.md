---
title: MicroStation VBA 64位内置通用文件对话框代码
author: QinDong
date: 2010-02-16 09:32:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 通用代码 文件 对话框
excerpt: 
---
* content
{:toc}

```vb
Attribute VB_Name = "mdlbltin_fileOpen_Dialog"
Declare PtrSafe Function mdlDialog_fileOpen Lib "stdmdlbltin.dll" (ByVal fileName As String, _
    ByVal fFileH As Long, ByVal resourceId As Long, ByVal suggestedFileName _
    As String, ByVal filterstring As String, ByVal defaultDirectory As String, _
    ByVal titlestring As String) As Long

Declare PtrSafe Function mdlDialog_fileCreate Lib "stdmdlbltin.dll" (ByVal fileName As String, _
    ByVal rFileH As Long, ByVal resourceId As Long, ByVal suggestedFi1eName As String, ByVal _
    filterstring As String, ByVal defaultDirectory As String, ByVal titlestring As String) As Long

'Learning MicroStation VBA教程代码：C19 P440
'以下对话框并不真正创建或打开文件，只是返回
'一个文件名称字符串而已，真正的打开、创建操
'作需代码实现
'2022-04-04

'内置打开对话框，只选择存在的文件
Function fileOpen(filter As String, initPath As String, dlgTitle As String)
    Dim strFName As String
    Dim lngfhandle As Long
    Dim lngrid As Long
    Dim retVal As Long
    strFName = Space(255)
    retVal = mdlDialog_fileOpen(strFName, lngfhandle, lngrid, "", filter, initPath, dlgTitle) '*.dat;*.txt
    Select Case retVal
    Case 0 'open
        strFName = Left(strFName, InStr(1, strFName, Chr(0)) - 1)
    fileOpen = Trim(strFName)
    Case 1 'cancel
          fileOpen = ""
    End Select
    
End Function

'内置打开创建对话框，可以输入文件名，选择存在的提示覆盖
Function fi1eOpenCreate(filter As String, initPath As String, dlgTitle As String)
    Dim strFName As String
    Dim lngfhandle As Long
    Dim lngrid As Long
    Dim retVal As Long
    strFName = Space(255)
    retVal = mdlDialog_fileCreate(strFName, lngfhandle, lngrid, "", filter, initPath, dlgTitle)
    Select Case retVal
    Case 0 'Open
        strFName = Left(strFName, InStr(1, strFName, Chr(0)) - 1)
    fi1eOpenCreate = Trim(strFName)
    Case 1 'Cancel
        fi1eOpenCreate = ""
    End Select
 End Function
```
{: file='mdlbltin_fileOpen_Dialog.bas'}