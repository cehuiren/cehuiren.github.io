---
layout: post
title:  "MicroStation XM 断面处理程序源码-窗体-AddModelFrm"
date: 2008-05-26
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>窗体：AddModelFrm.frm

```vb
VERSION 5.00
Begin {C62A69F0-16DC-11CE-9E98-00AA00574A4F} AddModelFrm 
   Caption         =   "添加断面 (C)SurMap.com 覃东"
   ClientHeight    =   855
   ClientLeft      =   45
   ClientTop       =   435
   ClientWidth     =   4575
   OleObjectBlob   =   "AddModelFrm.frx":0000
   ShowModal       =   0   'False
   StartUpPosition =   1  '所有者中心
End
Attribute VB_Name = "AddModelFrm"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = True
Attribute VB_Exposed = False
Private Sub AddCom_Click()
Dim strModelDescrip As String

If Not FindModel(Trim(adModelName)) Then
        strModelDescrip = "桩  号:" & strModelName & vbCrLf & "创建于:" & Now & vbCrLf & "创建者:" & vbCrLf & vbCrLf & "操作记录:"
        Set m = ActiveDesignFile.Models.Add(ActiveDesignFile.DefaultModelReference, Trim(adModelName), strModelDescrip, msdModelTypeNormal, False)
    ViewModelFrm.DescriptionTxt.Text = ActiveModelReference.Description
    ViewModelFrm.ComboBox1.AddItem Trim(adModelName)
    ViewModelFrm.ComboBox1.Text = Trim(adModelName)
    ViewModelFrm.cmdPrevious.Enabled = True
    ViewModelFrm.cmdNext.Enabled = True
    ViewModelFrm.ComboBox1.Enabled = True
    ViewModelFrm.DelModelCom.Enabled = True
    ViewModelFrm.SaveInfoCmd.Enabled = True
Me.Hide
End If
End Sub
```