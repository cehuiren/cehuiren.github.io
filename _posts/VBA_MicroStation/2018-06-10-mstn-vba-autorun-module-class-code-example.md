---
title: MicroStation VBA AutoRun
author: QinDong
date: 2018-06-10 11:00:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 自动运行
excerpt: 
---
* content
{:toc}

A common question asked by new VBA developers is how to automatically start a macro when you start MicroStation. There are several steps involved in this process.

1. Set your VBA project to autoload when MicroStation starts. This can be done by either adding the name of your VBA project to the MS_VBAAUTOLOADPROJECTS workspace variable, or by simply checking the Auto-Load column in the VBA Project Manager dialog.

2. In your code module you must create a sub named OnProjectLoad. This sub will be executed when your macro is loaded by MicroStation.

3. The VBA is loaded prior to the MicroStation Manager dialog being displayed, or any file being opened. This means you must add in an event handler to monitor for file open and close events so that the VBA will know when MicroStation has opened a file.

4. After the VBA knows a file is open you can call other Sub and/or Function routines to operate on the file.
The example code modules below show the required subs to automatically run a VBA macro.

### Module Code
```vb
Dim myOpen As clsFileOpenClose
Sub OnProjectLoad()
'
' Autorun macro code goes here
'
    MsgBox "Your macro is loaded – Welcome to MicroStation!"
    MonitorFileOpenClose
End Sub

Sub MonitorFileOpenClose()
    Set myOpen = New clsFileOpenClose
End Sub
```

### Class Module Code - clsFileOpenClose
```vb
Dim WithEvents objApp As Application
Private isFirstFile As Boolean

Private Sub Class_Initialize()
    Set objApp = Application
    isFirstFile = True
End Sub

Private Sub objApp_OnDesignFileClosed(ByVal FileName As String)
    MsgBox "Closing file " & FileName, vbInformation
End Sub

Private Sub objApp_OnDesignFileOpened(ByVal FileName As String)
'
' Do something different if this is the first file
'
    If isFirstFile Then
        MsgBox "Your first file is open: " & FileName  & vbCrLf & "Have a great CAD experience!", vbInformation isFirstFile = False
    Else
        MsgBox "Opening file " & FileName, vbInformation
    End If
'
' Call your automatic processing sub/function code from here
'
End Sub
```
{: .nolineno }