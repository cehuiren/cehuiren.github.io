---
layout: post
title:  "MicroStation XM 断面处理程序源码-模块-DrawDesignLineModel"
date: 2008-03-10
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>模块名：DrawDesignLineModel.bas

```vb
Attribute VB_Name = "DrawDesignLineModel"
Public curPoint As Point3d
Public curColor As Integer
Public curLineStyle As LineStyle
Public curWeight As Integer
Public curLevel As Level
Public curScale As Double  '比例尺分母
Public onPaperSize As Integer '标注在图尺上的打印尺寸,毫米为单位
Public preStr As String '高程标注前缘
Public KeyInChr As String '判断输入的字符
Public comRows As Long '当前命令行
Dim csCal As New CSExpressionCalculator.Calculator


'读取命令或命令的参数
Function readCommand(cmd As String, pos As Integer) As String
Dim rcmd
rcmd = Split(cmd, " ") '一个空格分隔命令与参数
If pos <= UBound(rcmd) + 1 And pos > 0 Then
readCommand = rcmd(pos - 1)
Else
readCommand = ""
End If
End Function


'读取命令指定参数
Function readPara(para As String, pos As Integer) As String
Dim rpara
rpara = Split(para, ",")
If pos <= UBound(rpara) + 1 And pos > 0 Then
readPara = rpara(pos - 1)
Else
readPara = ""
End If
End Function

'读取命令指定参数
Function readParaProfile(para As String, pos As Integer) As String
Dim rpara
rpara = Split(para, ",")
If pos <= UBound(rpara) + 1 And pos > 0 Then
readParaProfile = rpara(pos - 1)
Else
readParaProfile = "-666666.666"
End If
If Trim(readParaProfile = "") Then readParaProfile = "-666666.666"
End Function


Sub CreateLine(spoint As Point3d, epoint As Point3d)
Dim Lpoints(1) As Point3d
Dim cLine As LineElement

Lpoints(0) = spoint
Lpoints(1) = epoint

Set cLine = CreateLineElement1(Nothing, Lpoints)
cLine.Level = curLevel
cLine.Color = curColor
cLine.LineStyle = curLineStyle
cLine.LineWeight = curWeight
ActiveModelReference.AddElement cLine
cLine.Rewrite
cLine.Redraw

End Sub

Sub CreateShortLine(spoint As Point3d, epoint As Point3d, LevelName As String)
Dim Lpoints(1) As Point3d
Dim cLine As LineElement

Lpoints(0) = spoint
Lpoints(1) = epoint

Set cLine = CreateLineElement1(Nothing, Lpoints)
cLine.Level = FindLevel(LevelName)
cLine.Color = curColor
cLine.LineStyle = ActiveDesignFile.LineStyles("0")
cLine.LineWeight = curWeight
ActiveModelReference.AddElement cLine
cLine.Rewrite
cLine.Redraw

End Sub


Function dms2rad(dms As String) As Double
Dim tmp, td As Integer, tm As Integer, ts As Single
tmp = Split(dms, ".")
td = tmp(0)
If UBound(tmp) = 1 Then
tm = Int(tmp(1) / 100)
ts = (tmp(1) / 100 - tm) * 100
End If
dms2rad = (td + tm / 60 + ts / 3600) / 180 * (Atn(1) * 4)

End Function

Function doComm(comString As String) As Boolean
Dim comStr As String
Dim comPara As String
Dim comStrTmp As String
Dim toPoint As Point3d


    Dim DltX As Double
    Dim DltY As Double
    Dim toH As Double
    Dim slope As String
    
'函数计算
Dim fxStr As String
Dim resultStr As String
Dim xFromVal As Double
Dim xToVal As Double
Dim xStepVal As Double
Dim xFromStr As String
Dim xToStr As String
Dim xStepStr As String
Dim cal_I As Double
Dim codeStr As String
    
    
comStrTmp = comString

Do While InStr(comStrTmp, "  ") <> 0
comStrTmp = Replace(comStrTmp, "  ", " ")
Loop

comStr = UCase(readCommand(comStrTmp, 1)) '读取命令
comPara = readCommand(comStrTmp, 2)       '命令中的参数
comPara = Replace(UCase(comPara), "%X%", curPoint.X)
comPara = Replace(UCase(comPara), "%Y%", curPoint.Y)


Select Case comStr
Case "S" '设计起点
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数不正确 语法：S 10,20"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数不正确 语法：S 10,20"
    Exit Function
    End If
    
    
    curPoint.X = Calc(readPara(comPara, 1))
    curPoint.Y = Calc(readPara(comPara, 2))
Case "H" '水平线
    Dim Hstr As String
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数不正确 语法：H 10"
    Exit Function
    End If
    
    toPoint.X = curPoint.X + Calc(readPara(comPara, 1))
    toPoint.Y = curPoint.Y
    CreateLine curPoint, toPoint
    Hstr = Format(toPoint.Y, "#.0##")
    CrtHText Hstr, curPoint, toPoint
    CrtDText curPoint, toPoint
    curPoint.X = toPoint.X
    curPoint.Y = toPoint.Y
Case "V" '竖线
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数不正确 语法：V 10"
    Exit Function
    End If

    toPoint.Y = curPoint.Y + Calc(readPara(comPara, 1))
    toPoint.X = curPoint.X
    CreateLine curPoint, toPoint
    curPoint.X = toPoint.X
    curPoint.Y = toPoint.Y
Case "I" '坡比
    'Dim toH As Double
    'Dim slope As String
    Dim slope_h As Double  '坡比分子
    Dim slope_d As Double  '坡比分母
    'Dim dltX As Double
    'Dim dltY As Double
    Dim op As String
    Dim tmp
    '标注坡比
    Dim biaoAng As Double
    Dim biaoStr As String
    Dim biaoPoint As Point3d
    
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数1不正确 语法：I 1/0.5,600"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数2不正确 语法：I 1/0.5,600"
    Exit Function
    End If
    
    
    op = readPara(comPara, 3)
    toH = Calc(readPara(comPara, 2))
    slope = readPara(comPara, 1)
    If op = "" Then op = "R"
    If UCase(op) = "L" Or UCase(op) = "R" Then
        If InStr(slope, "/") <> 0 Then '判断是否是1/20格式
            '用 1:20表求的坡比
            tmp = Split(slope, "/")
            If UBound(tmp) = 1 Then
            slope_h = tmp(0)
            slope_d = tmp(1)
            
            DltY = toH - curPoint.Y
            DltX = DltY * slope_d / slope_h
            
            If UCase(op) = "L" Then
            DltX = 0 - Abs(DltX)
            Else
            DltX = Abs(DltX)
            End If
            toPoint.X = curPoint.X + DltX
            toPoint.Y = toH
            
            CreateLine curPoint, toPoint
            
            If DltX > 0 And DltY < 0 Then biaoAng = -Atn(slope_h / slope_d)
            If DltX > 0 And DltY > 0 Then biaoAng = Atn(slope_h / slope_d)
            If DltX < 0 And DltY < 0 Then biaoAng = Atn(slope_h / slope_d)
            If DltX < 0 And DltY > 0 Then biaoAng = -Atn(slope_h / slope_d)
            
            'biaoStr = slope_h & ":" & Format(slope_d, "0.###")
            biaoStr = "1:" & Format(slope_d / slope_h, "0.###")
            biaoPoint.X = (curPoint.X + toPoint.X) / 2
            biaoPoint.Y = (curPoint.Y + toPoint.Y) / 2
            CrtText biaoStr, biaoPoint, biaoAng
            
            curPoint.X = toPoint.X
            curPoint.Y = toPoint.Y
            End If
        End If
    End If
Case "A"
'A D.MMSS,H
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数1不正确 语法：A 45.2235,600"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数2不正确 语法：A 45.2235,600"
    Exit Function
    End If

        toH = Calc(readPara(comPara, 2))
            slope = readPara(comPara, 2)
            '指定角度
            DltY = toH - curPoint.Y
            DltX = DltY / Tan(dms2rad(slope))
            toPoint.X = curPoint.X + DltX
            toPoint.Y = toH
            
            CreateLine curPoint, toPoint
            curPoint.X = toPoint.X
            curPoint.Y = toPoint.Y
Case "@"
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数1不正确 语法：@ 10,20"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数2不正确 语法：@ 10,20"
    Exit Function
    End If

    toPoint.X = curPoint.X + Calc(readPara(comPara, 1))
    toPoint.Y = curPoint.Y + Calc(readPara(comPara, 2))
    CreateLine curPoint, toPoint
    curPoint.X = toPoint.X
    curPoint.Y = toPoint.Y
Case "@P"
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数1不正确 语法：@ deltaX,Y"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数2不正确 语法：@ deltaX,Y"
    Exit Function
    End If

    toPoint.X = curPoint.X + Calc(readPara(comPara, 1))
    toPoint.Y = Calc(readPara(comPara, 2))
    CreateLine curPoint, toPoint
    curPoint.X = toPoint.X
    curPoint.Y = toPoint.Y
Case "P@"
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数1不正确 语法：P@ X,deltaY"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数2不正确 语法：P@ X,deltaY"
    Exit Function
    End If

    toPoint.X = Calc(readPara(comPara, 1))
    toPoint.Y = curPoint.Y + Calc(readPara(comPara, 2))
    CreateLine curPoint, toPoint
    curPoint.X = toPoint.X
    curPoint.Y = toPoint.Y

Case "P"
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数1不正确 语法：P 10,20"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数2不正确 语法：P 10,20"
    Exit Function
    End If

    toPoint.X = Calc(readPara(comPara, 1))
    toPoint.Y = Calc(readPara(comPara, 2))
    CreateLine curPoint, toPoint
    curPoint.X = toPoint.X
    curPoint.Y = toPoint.Y

Case "COLOR"
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数不正确 语法：Color 3"
    Exit Function
    End If
    Dim ctmp As Integer
    ctmp = readPara(comPara, 1)
    If ctmp >= 0 And ctmp <= 255 Then
    curColor = ctmp
    Else
    curColor = 0
    End If
Case "SCALE"
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数不正确 语法：SCALE 出图比例尺分母"
    Exit Function
    End If
    curScale = readPara(comPara, 1) / 1000
Case "LEVEL"
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数不正确 语法：Level 设计线"
    Exit Function
    End If
    Set curLevel = FindLevel(readPara(comPara, 1))
Case "WEIGHT"
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数不正确 语法：Weight 2"
    Exit Function
    End If
    Dim wtmp As Integer
    wtmp = readPara(comPara, 1)
    If wtmp >= 0 And wtmp <= 255 Then
    curWeight = wtmp
    Else
    curWeight = 0
    End If
Case "STYLE"
    Dim tmpLineStyle As LineStyle
    Dim tmpi As Long
    Dim Ltmp As Integer
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数不正确 语法：Style 3"
    Exit Function
    End If
    Ltmp = readPara(comPara, 1)
    
    tmpi = 0
    For Each tmpLineStyle In ActiveDesignFile.LineStyles
    If Ltmp = tmpi Then
    Set curLineStyle = tmpLineStyle
    Exit For
    Else
    Set curLineStyle = ByLevelLineStyle
    End If
    tmpi = tmpi + 1
    Next
Case "F(X)"
'读取函数表达式
fxStr = readPara(comPara, 1)
xFromStr = readPara(comPara, 2)
xToStr = readPara(comPara, 3)
xStepStr = readPara(comPara, 4)

If Trim(fxStr) <> "" And IsNumeric(Trim(xFromStr)) And IsNumeric(Trim(xToStr)) And IsNumeric(Trim(xStepStr)) Then
                xFromVal = Val(Trim(xFromStr))
                xToVal = Val(Trim(xToStr))
                xStepVal = Val(Trim(xStepStr))
            For cal_I = xFromVal To xToVal Step xStepVal
                codeStr = "x=" & cal_I & vbCrLf & Trim(fxStr)
                resultStr = csCal.Calculate(codeStr)
                If InStr(resultStr, "ERROR") = 0 Then
                    toPoint.X = cal_I
                    toPoint.Y = Val(resultStr)
                    CreateLine curPoint, toPoint
                    curPoint.X = toPoint.X
                    curPoint.Y = toPoint.Y
                Else
                    DesignLineFrm.Echo = "行：" & comRows & ":" & ToErrorStr(resultStr)
                End If
            Next
        Else
                    DesignLineFrm.Echo = "行：" & comRows & ":函数定义有误,语法:f(x) 函数表达式,变量初值,终值,步长"
        
    End If
Case "F(Y)"
'读取函数表达式
fxStr = readPara(comPara, 1)
xFromStr = readPara(comPara, 2)
xToStr = readPara(comPara, 3)
xStepStr = readPara(comPara, 4)

If Trim(fxStr) <> "" And IsNumeric(Trim(xFromStr)) And IsNumeric(Trim(xToStr)) And IsNumeric(Trim(xStepStr)) Then
                xFromVal = Val(Trim(xFromStr))
                xToVal = Val(Trim(xToStr))
                xStepVal = Val(Trim(xStepStr))
            For cal_I = xFromVal To xToVal Step xStepVal
                codeStr = "y=" & cal_I & vbCrLf & Trim(fxStr)
                resultStr = csCal.Calculate(codeStr)
                If InStr(resultStr, "ERROR") = 0 Then
                    toPoint.X = Val(resultStr)
                    toPoint.Y = cal_I
                    CreateLine curPoint, toPoint
                    curPoint.X = toPoint.X
                    curPoint.Y = toPoint.Y
                Else
                    DesignLineFrm.Echo = "行：" & comRows & ":" & ToErrorStr(resultStr)
                End If
            Next
        Else
                    DesignLineFrm.Echo = "行：" & comRows & ":函数定义有误,语法:f(x) 函数表达式,变量初值,终值,步长"
        
    End If
End Select

End Function

'创建坡比标注文本
Sub CrtText(str As String, midpoint As Point3d, rAng As Double)
Dim txtTmp As TextElement
Dim txtPoint As Point3d
Dim tSize As Double
tSize = onPaperSize * curScale

If rAng < 0 Then
txtPoint.X = midpoint.X - Len(str) * tSize / 2 * Abs(Cos(rAng)) + onPaperSize * curScale
txtPoint.Y = midpoint.Y + Len(str) * tSize / 2 * Abs(Sin(rAng)) + onPaperSize * curScale
Else
txtPoint.X = midpoint.X - Len(str) * tSize * Cos(rAng) / 2 - onPaperSize * curScale
txtPoint.Y = midpoint.Y - Len(str) * tSize * Sin(rAng) / 2 + onPaperSize * curScale
End If

Set txtTmp = CreateTextElement1(Nothing, str, txtPoint, Matrix3dFromAxisAndRotationAngle(2, rAng))
Set txtTmp.TextStyle = adTextStyle("设计线标注", "宋体", 0.4, 0.4)
txtTmp.TextStyle.Height = tSize
txtTmp.TextStyle.Width = tSize
txtTmp.TextStyle.Color = curColor
txtTmp.Level = FindLevel(curLevel.Name & "(坡比标注)")
ActiveModelReference.AddElement txtTmp
txtTmp.Rewrite
txtTmp.Redraw
End Sub


'创建高程标注文本
Sub CrtHText(str As String, spoint As Point3d, epoint As Point3d)
Dim txtTmp As TextElement
Dim txtPoint As Point3d
Dim strH As Double
Dim strW As Double
Dim tSize As Double
Dim strSize As Double
Dim txtPoint2 As Point3d

tSize = onPaperSize * curScale
strSize = (Len(str) + Len(preStr) - 1) * tSize




If dst(spoint, epoint) > strSize Then

txtPoint.X = (spoint.X + epoint.X - strSize) / 2
txtPoint.Y = (spoint.Y + epoint.Y) / 2 + tSize + curScale * 0.5

Set txtTmp = CreateTextElement1(Nothing, preStr & str, txtPoint, Matrix3dIdentity)
Set txtTmp.TextStyle = adTextStyle("设计线标注", "宋体", 0.4, 0.4)
txtTmp.TextStyle.Height = tSize
txtTmp.TextStyle.Width = tSize
txtTmp.TextStyle.Color = curColor
'txtTmp.Color = curColor
txtTmp.Level = FindLevel(curLevel.Name & "(高程标注)")
ActiveModelReference.AddElement txtTmp
txtTmp.Rewrite
txtTmp.Redraw

Else

If epoint.X > spoint.X Then
txtPoint.X = epoint.X + curScale * 3
txtPoint.Y = (spoint.Y + epoint.Y) / 2 + tSize + curScale * 0.5
Else
txtPoint.X = spoint.X + curScale * 5
txtPoint.Y = (spoint.Y + epoint.Y) / 2 + tSize + curScale * 0.5
End If


Set txtTmp = CreateTextElement1(Nothing, preStr & str, txtPoint, Matrix3dIdentity)
Set txtTmp.TextStyle = adTextStyle("设计线标注", "宋体", 0.4, 0.4)
txtTmp.TextStyle.Height = tSize
txtTmp.TextStyle.Width = tSize
txtTmp.TextStyle.Color = curColor
'txtTmp.Color = curColor
txtTmp.Level = FindLevel(curLevel.Name & "(高程标注)")
ActiveModelReference.AddElement txtTmp
txtTmp.Rewrite
txtTmp.Redraw
txtPoint.Y = spoint.Y
txtPoint2.X = txtPoint.X + strSize
txtPoint2.Y = spoint.Y
CreateShortLine txtPoint, txtPoint2, curLevel.Name & "(高程标注)"
End If

End Sub









'创建偏距标注文本
Sub CrtDText(spoint As Point3d, epoint As Point3d)
Dim txtTmp As TextElement
Dim txtPoint As Point3d
Dim strH As Double
Dim strW As Double
Dim tSize As Double
Dim strSize As Double
Dim txtPoint2 As Point3d
Dim str As String
tSize = onPaperSize * curScale
str = Format(epoint.X, "#.0##")
If epoint.X >= 0 Then
strSize = (Len(str) - 0.5) * tSize
Else
strSize = (Len(str) - 1) * tSize
End If



If dst(spoint, epoint) < 2 * tSize Then
If epoint.X > spoint.X Then
txtPoint.X = epoint.X + tSize + curScale
txtPoint.Y = epoint.Y - tSize
Else
txtPoint.X = spoint.X + tSize + curScale
txtPoint.Y = spoint.Y - tSize
End If


Set txtTmp = CreateTextElement1(Nothing, str, txtPoint, Matrix3dFromAxisAndRotationAngle(2, -Atn(1) * 2))
Set txtTmp.TextStyle = adTextStyle("设计线标注", "宋体", 0.4, 0.4)
txtTmp.TextStyle.Height = tSize
txtTmp.TextStyle.Width = tSize
txtTmp.TextStyle.Color = curColor
'txtTmp.Color = curColor
txtTmp.Level = FindLevel(curLevel.Name & "(偏距标注)")
ActiveModelReference.AddElement txtTmp
txtTmp.Rewrite
txtTmp.Redraw
txtPoint.X = txtPoint.X - tSize - curScale
txtPoint.Y = spoint.Y - tSize
txtPoint2.X = txtPoint.X
txtPoint2.Y = spoint.Y - strSize
CreateShortLine txtPoint, txtPoint2, curLevel.Name & "(偏距标注)"

Else

str = Format(spoint.X, "#.0##")
If spoint.X >= 0 Then
strSize = (Len(str) - 0.5) * tSize
Else
strSize = (Len(str) - 1) * tSize
End If
txtPoint.X = spoint.X + tSize + curScale
txtPoint.Y = spoint.Y - tSize
Set txtTmp = CreateTextElement1(Nothing, str, txtPoint, Matrix3dFromAxisAndRotationAngle(2, -Atn(1) * 2))
Set txtTmp.TextStyle = adTextStyle("设计线标注", "宋体", 0.4, 0.4)
txtTmp.TextStyle.Height = tSize
txtTmp.TextStyle.Width = tSize
txtTmp.TextStyle.Color = curColor
'txtTmp.Color = curColor
txtTmp.Level = FindLevel(curLevel.Name & "(偏距标注)")
ActiveModelReference.AddElement txtTmp
txtTmp.Rewrite
txtTmp.Redraw
txtPoint.X = txtPoint.X - tSize - curScale
txtPoint.Y = spoint.Y - tSize
txtPoint2.X = txtPoint.X
txtPoint2.Y = spoint.Y - strSize
CreateShortLine txtPoint, txtPoint2, curLevel.Name & "(偏距标注)"

str = Format(epoint.X, "#.0##")
If epoint.X >= 0 Then
strSize = (Len(str) - 0.5) * tSize
Else
strSize = (Len(str) - 1) * tSize
End If
txtPoint.X = epoint.X + tSize + curScale
txtPoint.Y = epoint.Y - tSize
Set txtTmp = CreateTextElement1(Nothing, str, txtPoint, Matrix3dFromAxisAndRotationAngle(2, -Atn(1) * 2))
Set txtTmp.TextStyle = adTextStyle("设计线标注", "宋体", 0.4, 0.4)
txtTmp.TextStyle.Height = tSize
txtTmp.TextStyle.Width = tSize
txtTmp.TextStyle.Color = curColor
'txtTmp.Color = curColor
txtTmp.Level = FindLevel(curLevel.Name & "(偏距标注)")
ActiveModelReference.AddElement txtTmp
txtTmp.Rewrite
txtTmp.Redraw
txtPoint.X = txtPoint.X - tSize - curScale
txtPoint.Y = spoint.Y - tSize
txtPoint2.X = txtPoint.X
txtPoint2.Y = spoint.Y - strSize
CreateShortLine txtPoint, txtPoint2, curLevel.Name & "(偏距标注)"


End If

End Sub


Function dst(ap As Point3d, bp As Point3d) As Double
dst = Sqr((bp.X - ap.X) ^ 2 + (bp.Y - ap.Y) ^ 2)
End Function

Function doCommForCal(comString As String) As Boolean
Dim comStr As String
Dim comPara As String
Dim comStrTmp As String
Dim toPoint As Point3d

    Dim DltX As Double
    Dim DltY As Double
    Dim toH As Double
    Dim slope As String
    
'函数计算
Dim fxStr As String
Dim resultStr As String
Dim xFromVal As Double
Dim xToVal As Double
Dim xStepVal As Double
Dim xFromStr As String
Dim xToStr As String
Dim xStepStr As String
Dim cal_I As Double
Dim codeStr As String
    
comStrTmp = comString

Do While InStr(comStrTmp, "  ") <> 0
comStrTmp = Replace(comStrTmp, "  ", " ")
Loop

comStr = UCase(readCommand(comStrTmp, 1)) '读取命令
comPara = readCommand(comStrTmp, 2)       '命令中的参数
comPara = Replace(UCase(comPara), "%X%", curPoint.X)
comPara = Replace(UCase(comPara), "%Y%", curPoint.Y)


Select Case comStr
Case "S" '设计起点
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数不正确 语法：S 10,20"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数不正确 语法：S 10,20"
    Exit Function
    End If
    
    
    curPoint.X = Calc(readPara(comPara, 1))
    curPoint.Y = Calc(readPara(comPara, 2))
    ListvCoor curPoint.X, curPoint.Y
Case "H" '水平线
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数不正确 语法：H 10"
    Exit Function
    End If
    
    toPoint.X = curPoint.X + Calc(readPara(comPara, 1))
    toPoint.Y = curPoint.Y
    ListvCoor toPoint.X, toPoint.Y
    curPoint.X = toPoint.X
    curPoint.Y = toPoint.Y
Case "V" '竖线
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数不正确 语法：V 10"
    Exit Function
    End If

    toPoint.Y = curPoint.Y + Calc(readPara(comPara, 1))
    toPoint.X = curPoint.X
    ListvCoor toPoint.X, toPoint.Y
    curPoint.X = toPoint.X
    curPoint.Y = toPoint.Y
Case "I" '坡比
    Dim slope_h As Double  '坡比分子
    Dim slope_d As Double  '坡比分母
    Dim op As String
    Dim tmp
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数1不正确 语法：I 1/0.5,600"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数2不正确 语法：I 1/0.5,600"
    Exit Function
    End If
    
    
    op = readPara(comPara, 3)
    toH = Calc(readPara(comPara, 2))
    slope = readPara(comPara, 1)
    If op = "" Then op = "R"
    If UCase(op) = "L" Or UCase(op) = "R" Then
        If InStr(slope, "/") <> 0 Then '判断是否是1/20格式
            '用 1:20表求的坡比
            tmp = Split(slope, "/")
            If UBound(tmp) = 1 Then
            slope_h = tmp(0)
            slope_d = tmp(1)
            
            DltY = toH - curPoint.Y
            DltX = DltY * slope_d / slope_h
            
            If UCase(op) = "L" Then
            DltX = 0 - Abs(DltX)
            Else
            DltX = Abs(DltX)
            End If
            toPoint.X = curPoint.X + DltX
            toPoint.Y = toH
            ListvCoor toPoint.X, toPoint.Y
            curPoint.X = toPoint.X
            curPoint.Y = toPoint.Y
            End If
        End If
    End If
Case "A"
'A D.MMSS,H
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数1不正确 语法：A 45.2235,600"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数2不正确 语法：A 45.2235,600"
    Exit Function
    End If

        toH = Calc(readPara(comPara, 2))
            slope = readPara(comPara, 2)
            '指定角度
            DltY = toH - curPoint.Y
            DltX = DltY / Tan(dms2rad(slope))
            toPoint.X = curPoint.X + DltX
            toPoint.Y = toH
            ListvCoor toPoint.X, toPoint.Y
            curPoint.X = toPoint.X
            curPoint.Y = toPoint.Y
Case "@"
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数1不正确 语法：@ 10,20"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数2不正确 语法：@ 10,20"
    Exit Function
    End If

    toPoint.X = curPoint.X + Calc(readPara(comPara, 1))
    toPoint.Y = curPoint.Y + Calc(readPara(comPara, 2))
    ListvCoor toPoint.X, toPoint.Y
    curPoint.X = toPoint.X
    curPoint.Y = toPoint.Y
Case "@P"
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数1不正确 语法：@ deltaX,Y"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数2不正确 语法：@ deltaX,Y"
    Exit Function
    End If

    toPoint.X = curPoint.X + Calc(readPara(comPara, 1))
    toPoint.Y = Calc(readPara(comPara, 2))
    ListvCoor toPoint.X, toPoint.Y
    curPoint.X = toPoint.X
    curPoint.Y = toPoint.Y
Case "P@"
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数1不正确 语法：P@ X,deltaY"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数2不正确 语法：P@ X,deltaY"
    Exit Function
    End If

    toPoint.X = Calc(readPara(comPara, 1))
    toPoint.Y = curPoint.Y + Calc(readPara(comPara, 2))
    ListvCoor toPoint.X, toPoint.Y
    curPoint.X = toPoint.X
    curPoint.Y = toPoint.Y

Case "P"
    '简单检查语法
    If readPara(comPara, 1) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数1不正确 语法：P 10,20"
    Exit Function
    End If
    '简单检查语法
    If readPara(comPara, 2) = "" Then
    DesignLineFrm.Echo = "行：" & comRows & " 参数2不正确 语法：P 10,20"
    Exit Function
    End If

    toPoint.X = Calc(readPara(comPara, 1))
    toPoint.Y = Calc(readPara(comPara, 2))
    ListvCoor toPoint.X, toPoint.Y
    curPoint.X = toPoint.X
    curPoint.Y = toPoint.Y
Case "F(X)"
'读取函数表达式
fxStr = readPara(comPara, 1)
xFromStr = readPara(comPara, 2)
xToStr = readPara(comPara, 3)
xStepStr = readPara(comPara, 4)

If Trim(fxStr) <> "" And IsNumeric(Trim(xFromStr)) And IsNumeric(Trim(xToStr)) And IsNumeric(Trim(xStepStr)) Then
                xFromVal = Val(Trim(xFromStr))
                xToVal = Val(Trim(xToStr))
                xStepVal = Val(Trim(xStepStr))
            For cal_I = xFromVal To xToVal Step xStepVal
                codeStr = "x=" & cal_I & vbCrLf & Trim(fxStr)
                resultStr = csCal.Calculate(codeStr)
                If InStr(resultStr, "ERROR") = 0 Then
                    toPoint.X = cal_I
                    toPoint.Y = Val(Format(resultStr, "0.0####"))
                    ListvCoor toPoint.X, toPoint.Y
                    curPoint.X = toPoint.X
                    curPoint.Y = toPoint.Y
                Else
                    DesignLineFrm.Echo = "行：" & comRows & ":" & ToErrorStr(resultStr)
                End If
            Next
        Else
                    DesignLineFrm.Echo = "行：" & comRows & ":函数定义有误,语法:f(x) 函数表达式,变量初值,终值,步长"
        
    End If
Case "F(X)"
'读取函数表达式
fxStr = readPara(comPara, 1)
xFromStr = readPara(comPara, 2)
xToStr = readPara(comPara, 3)
xStepStr = readPara(comPara, 4)

If Trim(fxStr) <> "" And IsNumeric(Trim(xFromStr)) And IsNumeric(Trim(xToStr)) And IsNumeric(Trim(xStepStr)) Then
                xFromVal = Val(Trim(xFromStr))
                xToVal = Val(Trim(xToStr))
                xStepVal = Val(Trim(xStepStr))
            For cal_I = xFromVal To xToVal Step xStepVal
                codeStr = "x=" & cal_I & vbCrLf & Trim(fxStr)
                resultStr = csCal.Calculate(codeStr)
                If InStr(resultStr, "ERROR") = 0 Then
                    toPoint.X = Val(Format(resultStr, "0.0###"))
                    toPoint.Y = cal_I
                    ListvCoor toPoint.X, toPoint.Y
                    curPoint.X = toPoint.X
                    curPoint.Y = toPoint.Y
                Else
                    DesignLineFrm.Echo = "行：" & comRows & ":" & ToErrorStr(resultStr)
                End If
            Next
        Else
                    DesignLineFrm.Echo = "行：" & comRows & ":函数定义有误,语法:f(x) 函数表达式,变量初值,终值,步长"
        
    End If
End Select
End Function

Sub ListvCoor(X As Double, Y As Double)
DesignLineFrm.vCoorList = DesignLineFrm.vCoorList & "," & X & "|" & Y
End Sub

Function calH(vp() As String, X As Double) As String
Dim i As Long
Dim iCount As Long
Dim xy
Dim xy2
Dim strTp As String
iCount = UBound(vp)
For i = 1 To iCount - 1
        xy = Split(vp(i), "|")
        xy2 = Split(vp(i + 1), "|")
    If (X > xy(0) And X < xy2(0)) Then
        strTp = X & vbTab & Format((xy2(1) - xy(1)) / (xy2(0) - xy(0)) * (X - xy(0)) + xy(1), "0.000")
        If InStr(calH, strTp) = 0 Then calH = calH & strTp & vbCrLf
    ElseIf (X < xy(0) And X > xy2(0)) Then
        strTp = X & vbTab & Format((xy(1) - xy2(1)) / (xy(0) - xy2(0)) * (X - xy2(0)) + xy2(1), "0.000")
        If InStr(calH, strTp) = 0 Then calH = calH & strTp & vbCrLf
    ElseIf X = xy(0) Then
        strTp = X & vbTab & Format(xy(1), "0.000")
        If InStr(calH, strTp) = 0 Then calH = calH & strTp & vbCrLf
    ElseIf X = xy2(0) Then
        strTp = X & vbTab & Format(xy2(1), "0.000")
        If InStr(calH, strTp) = 0 Then calH = calH & strTp & vbCrLf
    End If
Next
End Function

Function calX(vp() As String, Y As Double) As String
Dim i As Long
Dim iCount As Long
Dim xy
Dim xy2
Dim strTp As String
iCount = UBound(vp)
For i = 1 To iCount - 1
        xy = Split(vp(i), "|")
        xy2 = Split(vp(i + 1), "|")
    If (Y > xy(1) And Y < xy2(1)) Then
        strTp = Y & vbTab & Format((xy2(0) - xy(0)) / (xy2(1) - xy(1)) * (Y - xy(1)) + xy(0), "0.000")
        If InStr(calX, strTp) = 0 Then calX = calX & strTp & vbCrLf
    ElseIf (Y < xy(1) And Y > xy2(1)) Then
        strTp = Y & vbTab & Format((xy(0) - xy2(0)) / (xy(1) - xy2(1)) * (Y - xy2(1)) + xy2(0), "0.000")
        If InStr(calX, strTp) = 0 Then calX = calX & strTp & vbCrLf
    ElseIf Y = xy(1) Then
        strTp = Y & vbTab & Format(xy(0), "0.000")
        If InStr(calX, strTp) = 0 Then calX = calX & strTp & vbCrLf
    ElseIf Y = xy2(1) Then
        strTp = Y & vbTab & Format(xy2(0), "0.000")
        If InStr(calX, strTp) = 0 Then calX = calX & strTp & vbCrLf
    End If
Next
End Function

Private Function ToErrorStr(ByVal X As String) As String
'错误处理

If Mid(X, 1, 6) = "ERROR#" Then
Dim ErrorN As Integer
Dim ErrorStr As String
Dim N As Integer
Dim i As Integer
i = InStr(X, ",")
If i = 0 Then
ErrorN = Mid(X, 7)
N = -1
Else
ErrorN = Mid(X, 7, i - 7)
N = Val(Mid(X, i + 1))
End If
Select Case ErrorN
Case 1
ErrorStr = "缺少左括号("
Case 2
ErrorStr = "缺少右括号)"
Case 3
ErrorStr = "表达式中含有非法字符"
Case 4
ErrorStr = "表达式语法错误"
Case 5
ErrorStr = "无效的函数参数"
Case 6
ErrorStr = "溢出"
Case 7
ErrorStr = "表达式为空"
Case 8
ErrorStr = "类型不匹配"
Case 9
ErrorStr = "字符串引号不配对"
Case 10
ErrorStr = "缺少必选参数"
Case 11
ErrorStr = "除数为零"
Case 12
ErrorStr = "#号不配对"
Case 13
ErrorStr = "非法的日期"
Case 14
ErrorStr = "缺少期待的表达式"
Case 15
ErrorStr = "变量名非法"
Case 16
ErrorStr = "变量名太长"
Case 17
ErrorStr = "非法语句"
Case Else
ErrorStr = "未知错误"
End Select
If N > 0 Then
ToErrorStr = ErrorStr & "，在第" & Trim(N) & "句"
Else
ToErrorStr = ErrorStr
End If
Else
ToErrorStr = X
End If
End Function
```