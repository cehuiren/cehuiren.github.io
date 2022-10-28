---
layout: post
title:  "MicroStation XM 断面处理程序源码-模块-CalString"
date: 2008-03-12
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>模块名：CalString.bas

```vb
Attribute VB_Name = "CalString"
Option Explicit
Function Calc(ByVal Exp As String)
    Dim L As Integer, R As Integer, FN As String, Fv
    '将空格删除
    Exp = Replace(Exp, " ", "")
    Calc = CalCulNext(Exp)
End Function
Private Sub GetMatchExp(ByVal Exp As String, LF As String, RF As String, Lp As Integer, Rp As Integer)
    '从表达式中,查找匹配起始位置
    Dim strT As String
    Rp = InStr(Exp, RF)
    If Rp = 0 Then
        Lp = 0
    Else
        strT = Left(Exp, Rp - 1)
        strT = StrReverse(strT)
        Lp = Rp - InStr(strT, LF)
    End If
End Sub
Function CalCulNext(ByVal Exp As String) As String
    '本函数可以计算一个完整的表达式，调用前保证去掉所有空格
    '计算表达式中最优先的一步
    Dim Lp As Integer, Rp As Integer, Mp As Integer, FirstNum As String, SecondNum As String
    Call GetMatchExp(Exp, "(", ")", Lp, Rp)
    If Lp * Rp = 0 Then                     '没有括弧
        Mp = FirstOperater(Exp)             '可用的操作符位置
        If Mp = 0 Then                      '没有运算符
            CalCulNext = Exp
        Else
            FirstNum = GetOpNum(Exp, Mp, True)
            SecondNum = GetOpNum(Exp, Mp, False)
            CalCulNext = CalCulNext(Mid(Exp, 1, Mp - Len(FirstNum) - 1) & CStr(DirectCalValue(FirstNum, SecondNum, Mid(Exp, Mp, 1))) & Right(Exp, Len(Exp) - (Mp + Len(SecondNum))))
        End If
    Else                                    '有括弧
        FirstNum = Left(Exp, Lp - 1)
        SecondNum = Right(Exp, Len(Exp) - Rp)
        CalCulNext = CalCulNext(FirstNum & CalCulNext(Mid(Exp, Lp + 1, Rp - Lp - 1)) & SecondNum)
    End If
End Function
Private Function GetOpNum(ByVal Exp As String, ByVal OperPosition As Integer, ByVal AtLeft As Boolean) As String
'从表达式中取出指定操作符左或右侧的操作数
    Dim i As Integer, pStep As Integer, j As Integer, Ch As String
    j = OperPosition
    If AtLeft Then
        i = 1
        pStep = -1
    Else
        i = Len(Exp)
        pStep = 1
        j = j + 1
    End If
    For j = j + pStep To i Step pStep
        Ch = Mid(Exp, j, 1)
        If Not (Ch >= "0" And Ch <= "9" Or Ch = ".") Then   '
            i = j - pStep
            Exit For
        End If
    Next j
    If AtLeft Then
        j = OperPosition
        If i > 1 Then
            If Mid(Exp, i - 1, 1) = "-" Then
                If i = 2 Then                               '不是第一个字符,检查数字前有无负号
                    i = 1
                ElseIf i > 2 Then
                    Ch = Mid(Exp, i - 2, 1)
                    If Not (Ch >= "0" And Ch <= "9" Or Ch = ".") Then
                        i = i - 1
                    End If
                End If
            End If
        End If
    Else
        j = i + 1
        i = OperPosition + 1
    End If
    GetOpNum = Mid(Exp, i, j - i)
End Function
Private Function DirectCalValue(FirstV As String, SecondV As String, op As String)
'用操作符计算两个操作数
    Dim t1 As Double, t2 As Double
    t1 = Val(FirstV):   t2 = Val(SecondV)
    Select Case op
    Case "+"
        t1 = t1 + t2
    Case "-"
        t1 = t1 - t2
    Case "*"
        t1 = t1 * t2
    Case "/"
        t1 = t1 / t2
    Case Else
        Exit Function
    End Select
    DirectCalValue = CStr(Format(t1, "#.#######################"))
End Function
Private Function FirstOperater(ByVal Exp As String) As Integer
'返回指定表达式字符串中下一步可最先计算的操作符位置
    Dim P1 As Integer, P2 As Integer
    P1 = InStr(Exp, "*")
    P2 = InStr(Exp, "/")
    GoSub SubFun
    '从子程序返回,则没有 * / 符号
    P1 = InStr(Exp, "+")
    P2 = InStr(2, Exp, "-")
    '从子程序返回,则没有 + - 符号
    GoSub SubFun
    FirstOperater = 0
    Exit Function
SubFun:
    If P1 * P2 = 0 Then
        If P1 + P2 <> 0 Then
            FirstOperater = P1 + P2
            Exit Function
        End If
    Else
        If P1 > P2 Then P1 = P2
        FirstOperater = P1
        Exit Function
    End If
    Return
End Function
```