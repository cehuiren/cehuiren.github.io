---
title: VBA 64位通用文件夹对话框代码
author: QinDong
date: 2010-02-15 11:33:00 +0800
categories: 编程开发 VBA
tags: 通用代码 文件夹 对话框
excerpt: 
---
* content
{:toc}

```vb
Attribute VB_Name = "Dialog_64_Folder"
'选择文件夹(http://blog.sina.com.cn/s/blog_55fc7c2f0101mb4t.html)：
Option Explicit
'http://www.mvps.org/access/api/api0002.htm
'Code courtesy of Terry Kreft
        Private Type BROWSEINFO
            hOwner As LongPtr
            pidlRoot As LongPtr
            pszDisplayName As String
            lpszTitle As String
            ulFlags As Long
            lpfn As LongPtr
            lParam As LongPtr
            iImage As Long
        End Type
       
        Private Declare PtrSafe Function SHGetPathFromIDList Lib "shell32.dll" Alias "SHGetPathFromIDListA" (ByVal pidl As Long, ByVal pszPath As String) As Long
        Private Declare PtrSafe Function SHBrowseForFolder Lib "shell32.dll" Alias "SHBrowseForFolderA" (lpBrowseInfo As BROWSEINFO) As Long
Private Const BIF_RETURNONLYFSDIRS = &H1
Public Function BrowseDirectory(szDialogTitle As String) As String
On Error GoTo Err_BrowseDirectory
    Dim X As Long, bi As BROWSEINFO, dwIList As Long
    Dim szPath As String, wPos As Integer
   
    With bi
        '.hOwner = hWndAccessApp
        .lpszTitle = szDialogTitle
        .ulFlags = BIF_RETURNONLYFSDIRS
    End With
   
    dwIList = SHBrowseForFolder(bi)
    szPath = Space$(512)
    X = SHGetPathFromIDList(ByVal dwIList, ByVal szPath)
   
    If X Then
        wPos = InStr(szPath, Chr(0))
        BrowseDirectory = Left$(szPath, wPos - 1)
    Else
        BrowseDirectory = ""
    End If
Exit_BrowseDirectory:
    Exit Function
Err_BrowseDirectory:
    MsgBox Err.Number & " - " & Err.Description
    Resume Exit_BrowseDirectory
End Function
Public Sub TestOpeningDirectory()
On Error GoTo Err_TestOpeningDirectory
   
    Dim sDirectoryName As String
   
    sDirectoryName = BrowseDirectory("Find and select where to export the Excel report files.")
   
    If sDirectoryName <> "" Then MsgBox "You selected the '" & sDirectoryName & "' directory.", vbInformation
   
Exit_TestOpeningDirectory:
    Exit Sub
   
Err_TestOpeningDirectory:
    MsgBox Err.Number & " - " & Err.Description
    Resume Exit_TestOpeningDirectory
End Sub
```
{: file='CommonDialog_Folder_64.bas'}