---
layout: post
title:  "MicroStation XM 断面处理程序源码-模块-StartUpModel"
date: 2008-03-07
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>模块名：StartUpModel.bas

```vb
Attribute VB_Name = "StartUpModel"
'将读取CPU和硬盘序列号的Reg.DLL放在指定位置
Private Declare Function GetCPUID Lib "reg.mvba" () As String
Private Declare Function ReadPhysicalDrive Lib "reg.mvba" () As String
Private Declare Function CreateSerialNumber Lib "reg.mvba" () As String
Private Declare Function GetMD5Str Lib "reg.mvba" (ByVal str As String) As String
'Private Declare Function Des Lib "D:\SerDLL\reg.dll" () As Long

'The following code is from a Code Module. It simplycreates an instance of the class that has the WithEvents declaration.

Dim oOpenClose As clsOpenClose
Sub SetupHooks()
    Set oOpenClose = New clsOpenClose
End Sub

Public Sub OnProjectLoad()
Call SetupHooks
End Sub

Sub Init()
Dim CpuInfo As String
Dim HdInfo As String
Dim RegCodeStr As String
Dim MD5Str As String
Dim RegFilePath As String
Dim RegFile As New clsIniFile
Dim regStatus As Boolean
Dim ReadBit(12) As Integer
Dim i As Integer
Dim j As Integer
Dim RegChr As String


ReadBit(0) = 5
ReadBit(1) = 4
ReadBit(2) = 7
ReadBit(3) = 6
ReadBit(4) = 6
ReadBit(5) = 8

ReadBit(6) = 5
ReadBit(7) = 4
ReadBit(8) = 8
ReadBit(9) = 4
ReadBit(10) = 6
ReadBit(11) = 9
ReadBit(12) = 5


regStatus = False
SectionLoaded = False

CpuInfo = GetCPUID()
HdInfo = Trim(ReadPhysicalDrive())


MD5Str = GetMD5Str(CpuInfo & HdInfo)


If Right(Application.Path, 1) <> "\" Then
RegFilePath = Application.Path & "\"
Else
RegFilePath = Application.Path
End If

RegFile.INIFileName = RegFilePath & "regfile.ini"
RegCodeStr = RegFile.GetIniKey("注册信息", "注册码")

For i = 0 To UBound(ReadBit)
j = ReadBit(i)
RegChr = Mid(MD5Str, j, 1)
If Mid(RegCodeStr, i + 1, 1) = RegChr Then
regStatus = True
Else
regStatus = False
End If
Next

If regStatus Then
RegOk = True
'RegCom.Visible = False
Else
'RegCom.Visible = True
RegOk = False
End If

End Sub
```