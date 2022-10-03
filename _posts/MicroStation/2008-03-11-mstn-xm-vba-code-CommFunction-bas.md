---
layout: post
title:  "MicroStation XM 断面处理程序源码-模块-CommFunction"
date: 2008-03-11
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>模块名：CommFunction.bas

```vb
Attribute VB_Name = "CommFunction"
Public gAppName As String
Public RegOk As Boolean '注册状态
'编辑断面线属性时返回所选择的对象和状态
Public lpSelected As Boolean
Public lpLineElement As Element
Public lpLineID As String

'Public gScale As String
'Public gSide As Integer

'作用:查找层,若不存在则创建层
'返回值:层对象
'参数:层名,字符串类型
Function FindLevel(LvName As String) As Level
    Dim fLevel As Level
        Set fLevel = ActiveDesignFile.Levels.Find(LvName)
    If fLevel Is Nothing Then
        Set FindLevel = ActiveDesignFile.AddNewLevel(LvName)
        ActiveDesignFile.RewriteLevels
    Else
        Set FindLevel = fLevel
    End If
End Function

Sub ExportStage()
Dim epModel As ModelReference
Dim epFileNumber As Long
Dim epI As Long
epFileNumber = FreeFile
Open "C:\stage.txt" For Output As #epFileNumber
For Each epModel In ActiveDesignFile.Models
Print #epFileNumber, epModel.Name
Next
Close #epFileNumber
End Sub


Sub SetElementXdata(xdEle As Element, xdAppName As String, xdType As MsdXDatumType, xdValue)
Dim Xdata() As XDatum

If xdEle.HasAnyXData Then
Xdata = xdEle.GetXData(xdAppName)
End If

    'AppendXDatum Xdata, msdXDatumTypeControlString, "{"
    AppendXDatum Xdata, xdType, xdValue
    'AppendXDatum Xdata, msdXDatumTypeControlString, "}"

xdEle.SetXData xdAppName, Xdata
End Sub

'设置模型的扩展数据
Sub SetModelXdata(xdModel As ModelReference, xdAppName As String, xdType As MsdXDatumType, xdValue)
Dim Xdata() As XDatum
Dim xd_i As Integer
If xdModel.HasXData(xdAppName) Then
        Xdata = xdModel.GetXData(xdAppName)
        For xd_i = LBound(Xdata) To UBound(Xdata)
            With Xdata(xd_i)
                If .Type = xdType Then .Value = xdValue
            End With
        Next
Else
        'AppendXDatum Xdata, msdXDatumTypeControlString, "{"
        AppendXDatum Xdata, xdType, xdValue
        'AppendXDatum Xdata, msdXDatumTypeControlString, "}"
End If
xdModel.SetXData xdAppName, Xdata
End Sub

'得到模型的扩展数据
Function getModelXdata(xdModel As ModelReference, xdAppName As String, xdType As MsdXDatumType)
Dim Xdata() As XDatum
Dim xd_i As Integer
If xdModel.HasXData(xdAppName) Then
        Xdata = xdModel.GetXData(xdAppName)
        For xd_i = LBound(Xdata) To UBound(Xdata)
            With Xdata(xd_i)
                If .Type = xdType Then getModelXdata = .Value
            End With
        Next
Else
getModelXdata = ""
End If
End Function

Function LoadScalePara(ldModel As ModelReference, ldAppName As String) As String
Dim para
Dim strXdata As String
    strXdata = getModelXdata(ldModel, ldAppName, msdXDatumTypeString)
    If strXdata <> "" Then
            para = Split(strXdata, "|")
            LoadScalePara = para(0)
    Else
            LoadScalePara = ""
    End If
End Function

Function LoadSidePara(ldModel As ModelReference, ldAppName As String) As Integer
Dim para
Dim strXdata As String
    strXdata = getModelXdata(ldModel, ldAppName, msdXDatumTypeString)
    If strXdata <> "" Then
            para = Split(strXdata, "|")
            LoadSidePara = Val(para(1))
    Else
            LoadSidePara = 0
    End If
End Function

Function LoadGridPara(ldModel As ModelReference, ldAppName As String) As Boolean
Dim para
Dim strXdata As String
    strXdata = getModelXdata(ldModel, ldAppName, msdXDatumTypeString)
    If strXdata <> "" Then
            para = Split(strXdata, "|")
         If para(2) = "True" Then
          LoadGridPara = True
         Else
          LoadGridPara = False
         End If
    Else
    LoadGridPara = False
    End If
End Function

Function LoadFullGridPara(ldModel As ModelReference, ldAppName As String) As Boolean
Dim para
Dim strXdata As String
    strXdata = getModelXdata(ldModel, ldAppName, msdXDatumTypeString)
    If strXdata <> "" Then
            para = Split(strXdata, "|")
         If para(3) = "True" Then
          LoadFullGridPara = True
         Else
          LoadFullGridPara = False
         End If
    Else
    LoadFullGridPara = False
    End If
End Function

Function LoadPaperPara(ldModel As ModelReference, ldAppName As String) As String
Dim para
Dim strXdata As String
    strXdata = getModelXdata(ldModel, ldAppName, msdXDatumTypeString)
    If strXdata <> "" Then
            para = Split(strXdata, "|")
         If para(4) = "A4" Then LoadPaperPara = "A4"
         If para(4) = "A3" Then LoadPaperPara = "A3"
    Else
         LoadPaperPara = "A4"
    End If
End Function

Function LoadPaperWidthPara(ldModel As ModelReference, ldAppName As String) As String
Dim para
Dim strXdata As String
    strXdata = getModelXdata(ldModel, ldAppName, msdXDatumTypeString)
    If strXdata <> "" Then
            para = Split(strXdata, "|")
            LoadPaperWidthPara = para(5)
    Else
         LoadPaperWidthPara = "210"
    End If
End Function

Function LoadPaperHeightPara(ldModel As ModelReference, ldAppName As String) As String
Dim para
Dim strXdata As String
    strXdata = getModelXdata(ldModel, ldAppName, msdXDatumTypeString)
    If strXdata <> "" Then
            para = Split(strXdata, "|")
            LoadPaperHeightPara = para(6)
    Else
         LoadPaperHeightPara = "297"
    End If
End Function

Function LoadPaperOrientationPara(ldModel As ModelReference, ldAppName As String) As String
Dim para
Dim strXdata As String
    strXdata = getModelXdata(ldModel, ldAppName, msdXDatumTypeString)
    If strXdata <> "" Then
            para = Split(strXdata, "|")
            LoadPaperOrientationPara = para(7)
    Else
         LoadPaperOrientationPara = "H"
    End If
End Function
'确定选择的纸张
Sub DeterminePaper(dModel As ModelReference, dAppName As String, dPaperWidth As Integer, dPaperHeight As Integer)
Dim dpPaper As String
Dim dpPaperWidth As String
Dim dpPaperHeight As String
Dim dpPaperOrien As String

dpPaperWidth = LoadPaperWidthPara(dModel, dAppName)
dpPaperHeight = LoadPaperHeightPara(dModel, dAppName)
dpPaperOrien = LoadPaperOrientationPara(dModel, dAppName)

If dpPaperOrien = "H" Then
dPaperWidth = Val(dpPaperHeight)
dPaperHeight = Val(dpPaperWidth)
Else
dPaperWidth = Val(dpPaperWidth)
dPaperHeight = Val(dpPaperHeight)
End If
End Sub


Function MakeProfileLineID() As String
Randomize (Timer())
MakeProfileLineID = Format(Date, "yyyymmdd") & "_" & Format(Time(), "hhmm") & "_" & Int(Rnd() * 100000#)
End Function

Sub DelLine(dlModel As ModelReference, dlAppName As String, dlLineID As String)
Dim dlEe As ElementEnumerator
Set dlEe = dlModel.GraphicalElementCache.Scan
Do While dlEe.MoveNext
 If dlEe.Current.HasXData(dlAppName) And DetectEleID(dlEe.Current, dlAppName, dlLineID) Then
 dlModel.RemoveElement dlEe.Current
 End If
Loop
End Sub

Sub DelLineAll(dlAppName As String, dlLineID As String)
Dim dlEe As ElementEnumerator
Dim dlModel As ModelReference
For Each dlModel In ActiveDesignFile.Models
        Set dlEe = dlModel.GraphicalElementCache.Scan
    Do While dlEe.MoveNext
        If dlEe.Current.HasXData(dlAppName) And DetectEleID(dlEe.Current, dlAppName, dlLineID) Then
        dlModel.RemoveElement dlEe.Current
     End If
    Loop
Next
End Sub

'得到断面线的ID号
Function GetEleID(idEle As Element, idAppName As String) As String
Dim Id_i As Integer
Dim idXdata() As XDatum
  GetEleID = ""
  If idEle.HasXData(idAppName) Then
    idXdata = idEle.GetXData(idAppName)
    GetEleID = idXdata(0).Value
End If
End Function



Function DetectEleID(idEle As Element, idAppName As String, idEleID As String) As Boolean
Dim Id_i As Integer
Dim idXdata() As XDatum
  DetectEleID = False
  If idEle.HasXData(idAppName) Then
    idXdata = idEle.GetXData(idAppName)
    For Id_i = LBound(idXdata) To UBound(idXdata)
    If idXdata(Id_i).Value = idEleID Then DetectEleID = True
    Next
End If
End Function
'ViewModelFrm使用排序过程
Public Sub BubbleSort(arr() As String, numEls As Long, Optional Descending As Boolean = False)
    Dim Value As String
    Dim index As Long
    Dim firstItem As Long
    Dim indexLimit As Long
    Dim lastSwap As Long

    ' account for optional arguments
    firstItem = LBound(arr)
    lastSwap = numEls

    Do
        indexLimit = lastSwap - 1
        lastSwap = 0
        For index = firstItem To indexLimit
            Value = arr(index)
            If (Value > arr(index + 1)) Xor Descending Then
                ' if the items are not in order, swap them
                arr(index) = arr(index + 1)
                arr(index + 1) = Value
                lastSwap = index
            End If
        Next
    Loop While lastSwap
End Sub

'ViewModelFrm使用排序过程
Public Sub BubbleSortMath(arr() As String, numEls As Long, Optional Descending As Boolean = False)
    Dim Value As String
    Dim index As Long
    Dim firstItem As Long
    Dim indexLimit As Long
    Dim lastSwap As Long

    ' account for optional arguments
    firstItem = LBound(arr)
    lastSwap = numEls

    Do
        indexLimit = lastSwap - 1
        lastSwap = 0
        For index = firstItem + 1 To indexLimit
            Value = arr(index)
            If (ReplaceNotNumericChr(Value) > ReplaceNotNumericChr(arr(index + 1))) Xor Descending Then
                ' if the items are not in order, swap them
                arr(index) = arr(index + 1)
                arr(index + 1) = Value
                lastSwap = index
            End If
        Next
    Loop While lastSwap
End Sub

'ViewModelFrm使用排序过程
Public Sub BubbleCoorListSortMath(arr() As String, Optional Descending As Boolean = False)
    Dim Value As Double
    Dim Value2 As Double
    Dim ValueTmp As String
    Dim index As Long
    Dim numEls As Long
    Dim firstItem As Long
    Dim indexLimit As Long
    Dim lastSwap As Long
    Dim xy() As String
    Dim xy2() As String
    firstItem = LBound(arr)
    numEls = UBound(arr)
    lastSwap = numEls
    
    Do
        indexLimit = lastSwap - 1
        lastSwap = 0
        For index = firstItem + 1 To indexLimit
            xy = Split(arr(index), "|")
            Value = Val(xy(0))
            ValueTmp = arr(index)
            xy2 = Split(arr(index + 1), "|")
            Value2 = Val(xy2(0))
            If Value > Value2 Xor Descending Then
                arr(index) = arr(index + 1)
                arr(index + 1) = ValueTmp
                lastSwap = index
            End If
        Next
    Loop While lastSwap
End Sub


Function Azimuth(Sx As Double, Sy As Double, Ex As Double, Ey As Double)
Dim DltX As Double, DltY As Double
'Pi = 3.14159265358979 'Atn(1) * 4 '定义PI值
DltX = Ex - Sx
DltY = Ey - Sy + 1E-20
Azimuth = Pi() * (1 - Sgn(DltY) / 2) - Atn(DltX / DltY) '计算方位角
If Azimuth < 0 Then Azimuth = Azimuth + 2 * Pi()
End Function

'判断指定名称的模型是否存在,返回值为布尔型
Function FindModel(MdlName) As Boolean
    Dim i As Long
    Dim j As Long
    FindModel = False
    'Dim ModelArray() As String
    
    'Set OldModel = ActiveModelReference
        
    j = ActiveDesignFile.Models.Count
    'ReDim ModelArray(j)
    
    For i = 1 To j
      If ActiveDesignFile.Models(i).Name <> "Default" Then
            If IsNumeric(ActiveDesignFile.Models(i).Name) And IsNumeric(MdlName) Then
                 If Val(ActiveDesignFile.Models(i).Name) = Val(MdlName) Then FindModel = True
            Else
                 If Trim(ActiveDesignFile.Models(i).Name) = Trim(MdlName) Then FindModel = True
            End If
      End If
       ' ModelArray(i) = ActiveDesignFile.Models(i).Name
    Next
    
   ' BubbleSort ModelArray, j
    
   ' For i = 1 To j
'        ComboBox1.AddItem ActiveDesignFile.Models(i).Name
  '     ComboBox1.AddItem ModelArray(i)
    'Next
    
    'ComboBox1.Text = ActiveModelReference.Name
    'lblDescription.Caption = ActiveModelReference.Description

End Function

Function getElementXdata(xdElement As Element, xdAppName As String, xdType As MsdXDatumType)
Dim Xdata() As XDatum
Dim xd_i As Integer
If xdElement.HasXData(xdAppName) Then
        Xdata = xdElement.GetXData(xdAppName)
        For xd_i = LBound(Xdata) To UBound(Xdata)
            With Xdata(xd_i)
                If .Type = xdType Then getElementXdata = .Value
            End With
        Next
Else
getElementXdata = ""
End If
End Function
'将桩号中的非数字字符去掉后进行比较用
Function ReplaceNotNumericChr(srcStr As String) As Double
Dim srcStrTmp As String
Dim rslStr As String
Dim srcStrLen As Long
Dim rpIdx_i As Long
Dim replacedChr As String
srcStrTmp = srcStr
srcStrLen = Len(srcStrTmp)
For rpIdx_i = 1 To srcStrLen
replacedChr = Mid(srcStrTmp, rpIdx_i, 1)
If replacedChr <> "-" And replacedChr <> "." And replacedChr <> "0" And replacedChr <> "1" And replacedChr <> "2" And replacedChr <> "3" And replacedChr <> "4" And replacedChr <> "5" And replacedChr <> "6" And replacedChr <> "7" And replacedChr <> "8" And replacedChr <> "9" And replacedChr <> "^" Then srcStrTmp = Replace(srcStrTmp, replacedChr, "^")
Next
rslStr = Replace(srcStrTmp, "^", "")
If InStr(rslStr, "-") <> 0 And Left(Trim(rslStr), 1) <> "-" Then rslStr = Replace(rslStr, "-", "")
If rslStr <> "" Then
ReplaceNotNumericChr = rslStr
Else
ReplaceNotNumericChr = 10000000000#
End If
End Function
```