---
title: VBA 64位通用文件对话框代码
author: QinDong
date: 2010-02-13 10:36:00 +0800
categories: 编程开发 VBA
tags: 通用代码 文件 对话框
excerpt: 
---
* content
{:toc}

```vb
Attribute VB_Name = "Dialog_64_File"
' INSTRUCTIONS:
'
'         ( For a working example, open the Debug window  )
'         ( and enter tsGetFileFromUserTest.              )
'
'.All the arguments for the function are optional.  You may call it with no
'.arguments whatsoever and simply assign its return value to a variable of
'.the Variant type.  For example:
'.
'.   varFileName = tsGetFileFromUser()
'.
'.The function will return:
'.   the full path and filename selected or entered by the user, or
'.   Null if an error occurs or if the user presses Cancel.
'.
'.Optional arguments may include any of the following:
'. rlngFlags      : one or more of the tscFN* constants (declared below)
'.                  Combine multiple constants like this:
'.                   tscFNHideReadOnly Or tscFNFileMustExist
'. strInitialDir : the directory to display when dialog opens
'. strFilter     : a string containing any filters you want to use. Each
'.                 part must be separated by the vbNullChar. -example below
'. lngFilterIndex: a 1-based index indicating which filter to start with.
'. strDefaultExt : Extension to use if user does not enter one.
'. strFileName   : Default File to display in the File Name text box.
'. strDialogTitle: Caption to display in the dialog's title bar.
'. fOpenFile     : Boolean-True for the Open dialog, False for Save dialog.
'
' FILTER EXAMPLE: The filter must be a string containing two parts for each
'  filter.  The first part is the Description, it is what the user will see
'  in the Files of Type box, e.g. "Text Files (*.txt)".  The second part is
'  the actual filter, e.g. "*.txt".  Each part and each filter must be
'  separated by the vbNullChar.  For example, to provide two filters, one for
'  *.mdb files, and one for all files, use a statement like this:
'
'  strFilter = "Access (*.mdb)" & vbNullChar & "*.mdb" _
'   & vbNullChar & "All Files (*.*)" & vbNullChar & "*.*"
'
'  Then pass your strFilter variable as the strFilter argument for the call
'  to tsGetFileFromUser()
'
'.--------------------------------------------------------------------------
Option Explicit

Private Declare PtrSafe Function ts_apiGetOpenFileName Lib "comdlg32.dll" Alias "GetOpenFileNameA" (tsFN As tsFileName) As Boolean
Private Declare PtrSafe Function ts_apiGetSaveFileName Lib "comdlg32.dll" Alias "GetSaveFileNameA" (tsFN As tsFileName) As Boolean
Private Declare PtrSafe Function CommDlgExtendedError Lib "comdlg32.dll" () As Long

Private Type tsFileName
   lStructSize As Long
   hwndOwner As LongPtr
   hInstance As LongPtr
   strFilter As String
   strCustomFilter As String
   nMaxCustFilter As Long
   nFilterIndex As Long
   strFile As String
   nMaxFile As Long
   strFileTitle As String
   nMaxFileTitle As Long
   strInitialDir As String
   strTitle As String
   flags As Long
   nFileOffset As Integer
   nFileExtension As Integer
   strDefExt As String
   lCustData As Long
   lpfnHook As LongPtr
   lpTemplateName As String
End Type

' Flag Constants
Private Const tscFNAllowMultiSelect = &H200
Private Const tscFNCreatePrompt = &H2000
Private Const tscFNExplorer = &H80000
Private Const tscFNExtensionDifferent = &H400
Private Const tscFNFileMustExist = &H1000
Private Const tscFNPathMustExist = &H800
Private Const tscFNNoValidate = &H100
Private Const tscFNHelpButton = &H10
Private Const tscFNHideReadOnly = &H4
Private Const tscFNLongNames = &H200000
Private Const tscFNNoLongNames = &H40000
Private Const tscFNNoChangeDir = &H8
Private Const tscFNReadOnly = &H1
Private Const tscFNOverwritePrompt = &H2
Private Const tscFNShareAware = &H4000
Private Const tscFNNoReadOnlyReturn = &H8000
Private Const tscFNNoDereferenceLinks = &H100000

Public Function tsGetFileFromUser( _
    Optional ByRef rlngflags As Long = 0&, _
    Optional ByVal strInitialDir As String = "", _
    Optional ByVal strFilter As String = "All Files (*.*)" & vbNullChar & "*.*", _
    Optional ByVal lngFilterIndex As Long = 1, _
    Optional ByVal strDefaultExt As String = "", _
    Optional ByVal strFileName As String = "", _
    Optional ByVal strDialogTitle As String = "", _
    Optional ByVal fOpenFile As Boolean = True) As Variant
'On Error GoTo tsGetFileFromUser_Err
    
    Dim tsFN As tsFileName
    Dim strFileTitle As String
    Dim fResult As Boolean

    ' Allocate string space for the returned strings.
    strFileName = Left(strFileName & String(256, 0), 256)
    strFileTitle = String(256, 0)

    ' Set up the data structure before you call the function
    With tsFN
        .lStructSize = LenB(tsFN)
        '.hwndOwner = Application.hWndAccessApp
        .strFilter = strFilter
        .nFilterIndex = lngFilterIndex
        .strFile = strFileName
        .nMaxFile = Len(strFileName)
        .strFileTitle = strFileTitle
        .nMaxFileTitle = Len(strFileTitle)
        .strTitle = strDialogTitle
        .flags = rlngflags
        .strDefExt = strDefaultExt
        .strInitialDir = strInitialDir
        .hInstance = 0
        .strCustomFilter = String(255, 0)
        .nMaxCustFilter = 255
        .lpfnHook = 0
    End With
   
    ' Call the function in the windows API
    If fOpenFile Then
        fResult = ts_apiGetOpenFileName(tsFN)
    Else
        fResult = ts_apiGetSaveFileName(tsFN)
    End If


    ' If the function call was successful, return the FileName chosen
    ' by the user.  Otherwise return null.  Note, the CancelError property
    ' used by the ActiveX Common Dialog control is not needed.  If the
    ' user presses Cancel, this function will return Null.
    If fResult Then
        rlngflags = tsFN.flags
        tsGetFileFromUser = tsTrimNull(tsFN.strFile)
    Else
        tsGetFileFromUser = Null
    End If

End Function

' Trim Nulls from a string returned by an API call.
Private Function tsTrimNull(ByVal strItem As String) As String
On Error GoTo tsTrimNull_Err
    Dim I As Integer
   
    I = InStr(strItem, vbNullChar)
    If I > 0 Then
        tsTrimNull = Left(strItem, I - 1)
    Else
        tsTrimNull = strItem
    End If
    
tsTrimNull_End:
    On Error GoTo 0
    Exit Function


tsTrimNull_Err:
    Beep
    MsgBox Err.Description, , "Error: " & Err.Number _
    & " in function basBrowseFiles.tsTrimNull"
    Resume tsTrimNull_End

End Function


'--------------------------------------------------------------------------
' Project      : tsDeveloperTools
' Description  : An example of how you can call tsGetFileFromUser()
' Calls        :
' Accepts      :
' Returns      :
' Written By   : Carl Tribble
' Date Created : 05/04/1999 11:19:41 AM
' Rev. History :
' Comments     : This is provided merely as an example to the programmer
'                It may be safely deleted from production code.
'--------------------------------------------------------------------------


Public Sub tsGetFileFromUserTest()
On Error GoTo tsGetFileFromUserTest_Err
   
    Dim strFilter As String
    Dim lngFlags As Long
    Dim varFileName As Variant


'   strFilter = "Access (*.mdb)" & vbNullChar & "*.mdb" _
'    & vbNullChar & "All Files (*.*)" & vbNullChar & "*.*"
    strFilter = "All Files (*.*)" & vbNullChar & "*.*"


    lngFlags = tscFNPathMustExist Or tscFNFileMustExist Or tscFNHideReadOnly
   
    varFileName = tsGetFileFromUser( _
    fOpenFile:=True, _
    strFilter:=strFilter, _
    rlngflags:=lngFlags, _
    strDialogTitle:="GetFileFromUser Test (Please choose a file)")
   
    If IsNull(varFileName) Then
        Debug.Print "User pressed 'Cancel'."
    Else
        Debug.Print varFileName
        'Forms![Form1]![Text1] = varFileName
    End If


    If varFileName <> "" Then MsgBox "You selected the '" & varFileName & "' file.", vbInformation


tsGetFileFromUserTest_End:
    On Error GoTo 0
    Exit Sub


tsGetFileFromUserTest_Err:
    Beep
    MsgBox Err.Description, , "Error: " & Err.Number _
     & " in sub basBrowseFiles.tsGetFileFromUserTest"
    Resume tsGetFileFromUserTest_End

End Sub
```
{: file='CommonDialog_File_64.bas'}