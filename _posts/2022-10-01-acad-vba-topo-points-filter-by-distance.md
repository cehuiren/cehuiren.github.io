---
title: AutoCAD VBA地形点抽稀-窗体
author: QinDong
date: 2022-10-01 17:26:00 +0800
categories: 编程开发 VBA-AutoCAD
tags: 地形 代码
excerpt: 
---
* content
{:toc}

```
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


Function Distance(Sx As Double, Sy As Double, Ex As Double, Ey As Double, Precision As Integer) As Double
Dim DltX As Double, DltY As Double
DltX = Ex - Sx
DltY = Ey - Sy
Distance = Round(Sqr(DltX * DltX + DltY * DltY), Precision)
End Function

Sub filter()
    Dim Dia1 As Object, strr As String, PPath As String
    Dim filterDist As Integer '抽稀距离
    Dim pNum() As Long, pSign() As String, pX() As Double, pY() As Double, pH() As Double
    Dim Datums As Variant
    Dim RowIndex As Long
    Dim rIndex As Long, rIndex2 As Long, xa As Double, ya As Double, xb As Double, yb As Double
    RowIndex = 1
    filterDist = Sheet1.Cells(1, 6) '抽稀距离
    If filterDist = 0 Then filterDist = 2



    Set Dia1 = Application.FileDialog(msoFileDialogFilePicker)
    Dia1.Title = "版权所有(C) 中国电建一二·五联合体  覃东 QQ:61902475 Email:cehui@139.com V20160225"
    With Dia1
        .AllowMultiSelect = False '限制只能同时选择一个文件
        .Filters.Clear
        .Filters.Add "南方CASS格式", "*.dat", 1 '限制显示的文件类型
        .show
        For Each vrtSelectedItem In .SelectedItems
            PPath = vrtSelectedItem
        Next
    End With

    If Trim(PPath) <> "" Then
        Open PPath For Input As #1
            Do While Not EOF(1)
                Line Input #1, strr
                If Trim(strr) <> "" Then
                Datums = Split(strr, ",")
                If UBound(Datums) = 4 Then
                 Sheet1.Cells(RowIndex, 1) = RowIndex
                 Sheet1.Cells(RowIndex, 2) = ""
                 Sheet1.Cells(RowIndex, 3) = Datums(2)
                Sheet1.Cells(RowIndex, 4) = Datums(3)
                Sheet1.Cells(RowIndex, 5) = Datums(4)
                End If
                End If
               RowIndex = RowIndex + 1
           Loop
           Close #1
    End If

'点抽稀
    rIndex = 1
    rIndex2 = rIndex + 1
Do While Sheet1.Cells(rIndex, 1).Text <> ""
        If Trim(Sheet1.Cells(rIndex2, 2)) = "" Then
            xa = Sheet1.Cells(rIndex, 3)
            ya = Sheet1.Cells(rIndex, 4)
            rIndex2 = rIndex + 1
                Do While Sheet1.Cells(rIndex2, 1).Text <> ""
                        If (Abs(Sheet1.Cells(rIndex2, 3).Text - xa) < filterDist And Abs(Sheet1.Cells(rIndex2, 4).Text - ya) < filterDist) Then
                           If Distance(xa, ya, Sheet1.Cells(rIndex2, 3).Text, Sheet1.Cells(rIndex2, 4).Text, 3) < filterDist And Trim(Sheet1.Cells(rIndex, 2)) = "" And Trim(Sheet1.Cells(rIndex2, 2)) = "" Then
                           Sheet1.Cells(rIndex2, 2) = "T"
                           End If
                        End If
                    rIndex2 = rIndex2 + 1
                Loop
        
        End If
    rIndex = rIndex + 1
Loop

    If Trim(PPath) <> "" Then
        rIndex = 1
        RowIndex = 1
        Open Left(PPath, InStr(PPath, ".dat") - 1) & "-抽稀(" & filterDist & "m)-" & Replace(Date, "-", "") & "-" & Replace(Time, ":", "") & ".dat" For Output As #2
            Do While Trim(Sheet1.Cells(rIndex, 1)) <> ""
                 'Sheet1.Cells(5, 7) = rIndex & ":" & RowIndex
                 If Trim(Sheet1.Cells(rIndex, 2)) = "" Then
                 Print #2, RowIndex & ",," & Format(Sheet1.Cells(rIndex, 3), "0.000") & "," & Format(Sheet1.Cells(rIndex, 4), "0.000") & "," & Format(Sheet1.Cells(rIndex, 5), "0.000")
                 RowIndex = RowIndex + 1
                 End If
               rIndex = rIndex + 1
           Loop
           Close #2
    End If

Sheet1.Range("B1:B10000").ClearContents

End Sub

Private Sub btn_Filter_Click()
'On Error GoTo tsGetFileFromUserTest_Err
    Dim filterDist As Integer '抽稀距离
    Dim pNum() As Long, pSign() As String, pX() As Double, pY() As Double, pH() As Double
    Dim Datums As Variant
    Dim RowIndex As Long, strr As String
    Dim rIndex As Long, rIndex2 As Long, xa As Double, ya As Double, xb As Double, yb As Double
    Dim strFilter As String
    Dim lngFlags As Long
    Dim varFileName As Variant
    Dim totalPoints As Long, effPoints As Long, delPoints As Long '总点数，有效点数
    RowIndex = 1
    If IsNumeric(Trim(txt_FilterDist.Text)) Then
        If Trim(txt_FilterDist.Text) > 0 Then
             filterDist = Trim(txt_FilterDist.Text)
        Else
            filterDist = 2
        End If
    Else
        filterDist = 2
    End If

'   strFilter = "Access (*.mdb)" & vbNullChar & "*.mdb" _
'    & vbNullChar & "All Files (*.*)" & vbNullChar & "*.*"
    strFilter = "CASS格式数据(*.dat)" & vbNullChar & "*.dat"


    lngFlags = tscFNPathMustExist Or tscFNFileMustExist Or tscFNHideReadOnly
   
    varFileName = tsGetFileFromUser( _
    fOpenFile:=True, _
    strFilter:=strFilter, _
    rlngflags:=lngFlags, _
    strDialogTitle:="打开文件 中国电建两河口水电站 覃东")
   
If varFileName <> "" Then
    txt_DatFileName.Text = varFileName
            Open varFileName For Input As #1
            Do While Not EOF(1)
                Line Input #1, strr
                If Trim(strr) <> "" Then
                Datums = Split(strr, ",")
                        If UBound(Datums) = 4 Then
                        ReDim Preserve pNum(RowIndex)
                        ReDim Preserve pSign(RowIndex)
                        ReDim Preserve pX(RowIndex)
                        ReDim Preserve pY(RowIndex)
                        ReDim Preserve pH(RowIndex)
                        pNum(RowIndex - 1) = RowIndex
                        pSign(RowIndex - 1) = ""
                        pX(RowIndex - 1) = Datums(2)
                        pY(RowIndex - 1) = Datums(3)
                        pH(RowIndex - 1) = Datums(4)
                    End If
                End If
               RowIndex = RowIndex + 1
           Loop
           Close #1
End If
            totalPoints = RowIndex - 1
           lbl_points.Caption = totalPoints

'点抽稀
    rIndex = 1
    rIndex2 = rIndex + 1
    delPoints = 0
Do While pNum(rIndex - 1) <> 0
        If Trim(pSign(rIndex2 - 1)) = "" Then
            xa = pX(rIndex - 1)
            ya = pY(rIndex - 1)
            rIndex2 = rIndex + 1
                Do While pNum(rIndex2 - 1) <> 0
                        If (Abs(pX(rIndex2 - 1) - xa) < filterDist And Abs(pY(rIndex2 - 1) - ya) < filterDist) Then
                           If Distance(xa, ya, pX(rIndex2 - 1), pY(rIndex2 - 1), 3) < filterDist And Trim(pSign(rIndex - 1)) = "" And Trim(pSign(rIndex2 - 1)) = "" Then
                           pSign(rIndex2 - 1) = "T"
                           delPoints = delPoints + 1
                           End If
                        End If
                    rIndex2 = rIndex2 + 1
                Loop
        
        End If
    rIndex = rIndex + 1
Loop

        lbl_filtered.Caption = totalPoints & "/" & delPoints
'保存
    If Trim(varFileName) <> "" Then
        rIndex = 1
        RowIndex = 1
        Open Left(varFileName, InStr(varFileName, ".dat") - 1) & "-抽稀(" & filterDist & "m)-" & Replace(Date, "-", "") & "-" & Replace(Time, ":", "") & ".dat" For Output As #2
            Do While Trim(pNum(rIndex - 1)) <> 0
                 If Trim(pSign(rIndex - 1)) = "" Then
                 Print #2, RowIndex & ",," & Format(pX(rIndex - 1), "0.000") & "," & Format(pY(rIndex - 1), "0.000") & "," & Format(pH(rIndex - 1), "0.000")
                 RowIndex = RowIndex + 1
                 End If
               rIndex = rIndex + 1
           Loop
           Close #2
    End If
               effPoints = RowIndex - 1
               lbl_epoints.Caption = effPoints
'清除数组
ReDim pNum(1)
ReDim pSign(1)
ReDim pX(1)
ReDim pY(1)
ReDim pH(1)

'tsGetFileFromUserTest_End:
'    On Error GoTo 0
'    Exit Sub
'
'
'tsGetFileFromUserTest_Err:
'    Beep
'    MsgBox Err.Description, , "Error: " & Err.Number _
'     & " in sub basBrowseFiles.tsGetFileFromUserTest"
'    Resume tsGetFileFromUserTest_End
End Sub

Private Sub UserForm_Click()

End Sub
```
{: file='Forms-frm_CassDatFilter'}