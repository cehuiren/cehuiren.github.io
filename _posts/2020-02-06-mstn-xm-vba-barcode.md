---
layout: post
title:  "MicroStation VBA创建横断面图水平、高程标尺代码"
date: 2020-02-06
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA 断面图 标尺 代码 XM 
excerpt: MicroStation VBA XM下绘横断面图标尺代码。
author: 测量老覃
---
* content
{:toc}

这是本人2006年左右在重庆巴山水电站时在MicroStation XM环境下编的断面处理工具中绘横断面标尺的一段代码，在MicroStation CE环境下需要作小的修改才能正常运行。其中多数功能是相同的，可以借鉴。

```vb
Dim miniX As Double 
Dim miniY As Double 
Dim maxX As Double 
Dim maxY As Double 
Sub DelSingleBar() 
Dim dbModel As ModelReference 
Dim dbEle As Element 
Dim dbEe As ElementEnumerator 
Set dbModel = ActiveModelReference 
Set dbEe = dbModel.GraphicalElementCache.Scan 
Do While dbEe.MoveNext 
Set dbEle = dbEe.Current 
 dbEle.IsLocked = False 
If dbEle.HasXData("(C)SurMap.com BarCreator") Then dbModel.RemoveElement dbEle 
Loop 
End Sub 
Sub DelAllBar() 
Dim dbModel As ModelReference 
Dim dbEle As Element 
Dim dbEe As ElementEnumerator 
For Each dbModel In ActiveDesignFile.Models 
Set dbEe = dbModel.GraphicalElementCache.Scan 
Do While dbEe.MoveNext 
Set dbEle = dbEe.Current 
 dbEle.IsLocked = False 
If dbEle.HasXData("(C)SurMap.com BarCreator") Then dbModel.RemoveElement dbEle 
Loop 
Next 
End Sub 
Sub CrtSingleBar() 
Dim cmModel As ModelReference 
Dim cmScale As String 
Dim cmSide As Integer 
Dim cmGrid As Boolean 
Dim cmPaperWidth As Integer 
Dim cmPaperHeight As Integer 
Dim cmFullGrid As Boolean 
cmGrid = LoadGridPara(ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter") 
cmScale = LoadScalePara(ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter") 
cmSide = LoadSidePara(ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter") 
If cmScale = "" Then cmScale = "500" 
If cmSide = 0 Then cmSide = -1 
gAppName = "(C)SurMap.com BarCreator" 
'For Each cmModel In ActiveDesignFile.Models 
Set cmModel = ActiveModelReference 
'If cmModel.Name <> "Default" Then 
ActiveDesignFile.Views(1).DisplaysFill = True 
ViewArea cmModel 
'调整图纸 
cmFullGrid = LoadFullGridPara(ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter") 
If cmGrid And cmFullGrid Then 
DeterminePaper ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter", cmPaperWidth, cmPaperHeight 
cmPaperWidth = cmPaperWidth - 60 
cmPaperHeight = cmPaperHeight - 50 
If cmPaperWidth / 10 * Val(cmScale) / 100 > (maxX - miniX) And cmPaperHeight / 10 * Val(cmScale) / 100 > (maxY - miniY) Then 
 miniX = miniX - (cmPaperWidth / 10 * Val(cmScale) / 100 - (maxX - miniX)) / 2 
 miniY = miniY - (cmPaperHeight / 10 * Val(cmScale) / 100 - (maxY - miniY)) / 2 
 maxX = miniX + cmPaperWidth / 10 * Val(cmScale) / 100 
 maxY = miniY + cmPaperHeight / 10 * Val(cmScale) / 100 
End If 
End If 
'调整图纸 
crtBar cmModel, miniX, miniY, maxX - miniX, maxY - miniY, Val(cmScale), cmSide, cmGrid, "标尺_" & cmScale, "宋体", 2, 2 
'crtBar cmModel, -135, 253, 200, 200, 500, 1, False, "标尺", "宋体", 2, 2 
'End If 
'Next 
End Sub 
Sub CrtAllBar() 
Dim cmModel As ModelReference 
Dim cmScale As String 
Dim cmSide As Integer 
Dim cmGrid As Boolean 
Dim cmPaperWidth As Integer 
Dim cmPaperHeight As Integer 
Dim cmFullGrid As BooleancmGrid = LoadGridPara(ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter") 
cmScale = LoadScalePara(ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter") 
cmSide = LoadSidePara(ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter") 
If cmScale = "" Then cmScale = "500" 
If cmSide = 0 Then cmSide = -1 
gAppName = "(C)SurMap.com BarCreator" 
For Each cmModel In ActiveDesignFile.Models 
If cmModel.Name <> "Default" And getModelXdata(cmModel, "surmap_dx", msdXDatumTypeString) <> "dxmodel" Then 
ViewArea cmModel 
'调整图纸 
cmFullGrid = LoadFullGridPara(ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter") 
If cmGrid And cmFullGrid Then 
DeterminePaper ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter", cmPaperWidth, cmPaperHeight 
cmPaperWidth = cmPaperWidth - 60 
cmPaperHeight = cmPaperHeight - 50 
If cmPaperWidth / 10 * Val(cmScale) / 100 > (maxX - miniX) And cmPaperHeight / 10 * Val(cmScale) / 100 > (maxY - miniY) Then 
 miniX = miniX - (cmPaperWidth / 10 * Val(cmScale) / 100 - (maxX - miniX)) / 2 
 miniY = miniY - (cmPaperHeight / 10 * Val(cmScale) / 100 - (maxY - miniY)) / 2 
 maxX = miniX + cmPaperWidth / 10 * Val(cmScale) / 100 
 maxY = miniY + cmPaperHeight / 10 * Val(cmScale) / 100 
End If 
End If 
'调整图纸 
crtBar cmModel, miniX, miniY, maxX - miniX, maxY - miniY, Val(cmScale), cmSide, cmGrid, "标尺_" & cmScale, "宋体", 2, 2 
'crtBar cmModel, -135, 253, 200, 200, 500, 1, False, "标尺", "宋体", 2, 2 
End If 
Next 
End Sub 
'创建标尺参数：左、右、宽、高、比例（整数分母）、左右侧（L or R）、网格（真或否）,字体样式名,字体名,字宽(毫米),字高(毫米), 
Sub crtBar(bModel As ModelReference, bLeft As Double, bBottom As Double, bWidth As Double, bHeight As Double, bScale As Integer, _ 
bSide As Integer, bGrid As Boolean, bTextStyleName As String, bFontName As String, bFontWidth_mm As Single, bFontHeight_mm As Single) 
Dim cbLevel As Level '绘标尺的层名 
Dim cbStep As Integer '步长 
Dim cbWeight As Single '竖标尺的宽度 
Dim cbLeft As Integer 
Dim cbBottom As Integer 
Dim cbWidth As Integer '宽度 
Dim cbHeight As Integer 
'循环次数 
Dim cbVtime As Integer 
Dim cbHtime As Integer 
Dim cbI As Integer 
Dim cbVpoints(3) As Point3d 
Dim cbModel As ModelReference 
Dim cbShp As ShapeElement 
Dim cbText As TextElement 
Dim cbLineEle As LineElement '水平线 
Dim cbLineStart As Point3d 
Dim cbLineEnd As Point3d 
Dim cbFontWidth As Single 
Dim cbFontHeight As Single 
'临时变量 
Dim cTmpLeft As Double 
Dim cTmpBottom As Double 
Dim cTmpRight As Double 
Dim cTmpUp As Double 
Dim cTmpVtag As Point3d '高程注记位置 
Dim cTmpHtag As Point3d '水平距离注记 
'断面桩号等信息 
Dim ProInfo As String 
Dim ProInfoPosition As Point3d 
'网格线 
Dim cTmpGridHStart As Point3d '水平 
Dim cTmpGridHEnd As Point3d 
Dim cTmpGridHLine As LineElement 
Dim cTmpGridVStart As Point3d '竖直 
Dim cTmpGridVEnd As Point3d 
Dim cTmpGridVLine As LineElement 
Set cbLevel = FindLevel("断面标尺") 
cbStep = Int(bScale / 100) 
cbWeight = 0.2 * cbStep 
cbLeft = (Int(bLeft / cbStep) - 1) * cbStepcbWidth = (Round(bWidth / cbStep, 0) + 2) * cbStep 
cbBottom = (Int(bBottom / cbStep) - 1) * cbStep 
cbHeight = (Round(bHeight / cbStep, 0) + 2) * cbStep 
cbVtime = cbHeight / cbStep 
cbHtime = cbWidth / cbStep 
'字高,将以mm为单位的字高换算为绘图单位 
cbFontWidth = bFontWidth_mm / 10 * cbStep 
cbFontHeight = bFontHeight_mm / 10 * cbStep 
Set cbModel = bModel 
For cbI = 1 To cbVtime 
If bSide <= 0 Then 
cTmpLeft = cbLeft 
cTmpRight = cTmpLeft + cbWeight 
Else 
cTmpLeft = cbLeft + cbWidth - cbWeight 
cTmpRight = cTmpLeft + cbWeight 
End If 
cTmpBottom = (cbI - 1) * cbStep + cbBottom 
cTmpUp = cTmpBottom + cbStep 
cbVpoints(0).X = cTmpLeft 
cbVpoints(0).Y = cTmpBottom 
cbVpoints(1).X = cTmpRight 
cbVpoints(1).Y = cTmpBottom 
cbVpoints(2).X = cTmpRight 
cbVpoints(2).Y = cTmpUp 
cbVpoints(3).X = cTmpLeft 
cbVpoints(3).Y = cTmpUp 
'======================= 
'竖标尺 
If cbI Mod 2 = 0 Then 
Set cbShp = CreateShapeElement1(Nothing, cbVpoints, msdFillModeFilled) 
Else 
Set cbShp = CreateShapeElement1(Nothing, cbVpoints, msdFillModeNotFilled) 
'竖标尺 
'======================= 
'*********************** 
'======================= 
'调整高程注记与竖标尺的间距在此 
'竖标尺注记 
cTmpVtag = cbVpoints(2) 
If bSide <= 0 Then 
cTmpVtag.X = cTmpVtag.X + 0.1 * cbStep '与竖标尺的间距,0.1为0.1公分,cbStep分图上一公分所对应的绘图长度 
Else 
cTmpVtag.X = cTmpVtag.X - 0.1 * cbStep - cbFontWidth * 3 '与竖标尺的间距,0.1为0.1公分,cbStep分图上一公分所对应的绘图长度 
End If 
cTmpVtag.Y = cTmpVtag.Y + cbFontHeight 
Set cbText = CreateTextElement1(Nothing, cTmpUp, cTmpVtag, Matrix3dIdentity) 
End If 
'调整高程注记与竖标尺的间距在此 
'竖标尺注记 
'======================= 
 cbShp.LineStyle = ActiveDesignFile.LineStyles("0") 
 cbShp.Level = cbLevel 
 cbShp.FillColor = 0 
 SetElementXdata cbShp, gAppName, msdXDatumTypeString, "Type:Bar" 
'锁定并禁止捕捉 
 cbShp.IsLocked = True 
 cbShp.IsSnappable = False 
 cbModel.AddElement cbShp 
 cbShp.Rewrite 
 cbShp.Redraw 
Set cbText.TextStyle = adTextStyle(bTextStyleName, bFontName, cbFontWidth, cbFontHeight) 
 cbText.Level = cbLevel 
 cbText.Color = 0 
 SetElementXdata cbText, gAppName, msdXDatumTypeString, "Type:Bar" 
'锁定并禁止捕捉 
' cbText.IsLocked = True 
' cbText.IsSnappable = False 
 cbModel.AddElement cbText 
 cbText.Rewrite 
 cbText.Redraw'网络线:水平线###################### 
If bGrid Then 
If bSide = -1 Then 
'标尺在左侧时 
 cTmpGridHStart.X = cTmpRight 
 cTmpGridHEnd.X = cTmpRight + cbWidth - cbWeight 
Else 
'标尺在右侧时 
 cTmpGridHStart.X = cbLeft 
 cTmpGridHEnd.X = cbLeft + cbWidth - cbWeight 
End If 
 cTmpGridHStart.Y = cTmpUp 
 cTmpGridHEnd.Y = cTmpUp 
Set cTmpGridHLine = CreateLineElement2(Nothing, cTmpGridHStart, cTmpGridHEnd) 
 cTmpGridHLine.LineStyle = ActiveDesignFile.LineStyles("0") 
 cTmpGridHLine.Level = cbLevel 
 cTmpGridHLine.Color = 64 
 SetElementXdata cTmpGridHLine, gAppName, msdXDatumTypeString, "Type:Bar" 
'锁定并禁止捕捉 
 cTmpGridHLine.IsLocked = True 
 cTmpGridHLine.IsSnappable = False 
 cbModel.AddElement cTmpGridHLine 
 cTmpGridHLine.Redraw 
 cTmpGridHLine.Rewrite 
End If 
'网络线:水平线###################### 
Next 
'********************** 
'水平标尺 
If bSide <= 0 Then 
'竖标尺在左边 
 cbLineStart.X = cbLeft + cbWeight 
 cbLineStart.Y = cbBottom 
 cbLineEnd.X = cbLeft + cbWidth 
 cbLineEnd.Y = cbBottom 
Else 
'竖标尺在右边 
 cbLineStart.X = cbLeft 
 cbLineStart.Y = cbBottom 
 cbLineEnd.X = cbLineStart.X + cbWidth - cbWeight 
 cbLineEnd.Y = cbBottom 
End If 
Set cbLineEle = CreateLineElement2(Nothing, cbLineStart, cbLineEnd) 
 SetElementXdata cbLineEle, gAppName, msdXDatumTypeString, "Type:Bar" 
 cbLineEle.LineStyle = ActiveDesignFile.LineStyles("0") 
 cbLineEle.Level = cbLevel 
'锁定并禁止捕捉 
 cbLineEle.IsLocked = True 
 cbLineEle.IsSnappable = False 
 cbModel.AddElement cbLineEle 
 cbLineEle.Rewrite 
 cbLineEle.Redraw 
'水平标尺 
'********************** 
For cbI = 1 To cbHtime 
If bSide <= 0 Then 
'左 
cbLineStart.X = cbLeft + cbI * cbStep 
cbLineEnd.X = cbLeft + cbI * cbStep 
Else 
'右 
cbLineStart.X = cbLeft + (cbI - 1) * cbStep 
cbLineEnd.X = cbLeft + (cbI - 1) * cbStep 
End If 
cbLineStart.Y = cbBottom 
cbLineEnd.Y = cbBottom + 0.2 * cbStep 
Set cbLineEle = CreateLineElement2(Nothing, cbLineStart, cbLineEnd) 
cbModel.AddElement cbLineEle 
SetElementXdata cbLineEle, gAppName, msdXDatumTypeString, "Type:Bar" 
'锁定并取消捕捉 
cbLineEle.Level = cbLevel 
cbLineEle.LineStyle = ActiveDesignFile.LineStyles("0") 
cbLineEle.IsLocked = True 
cbLineEle.IsSnappable = False 
cbLineEle.Rewrite 
cbLineEle.Redraw'水平距离注记位置 
cTmpHtag.X = cbLineStart.X - cbFontWidth 
cTmpHtag.Y = cbLineEnd.Y + 0.1 * cbStep + cbFontHeight 
Set cbText = CreateTextElement1(Nothing, cbLineStart.X, cTmpHtag, Matrix3dIdentity) 
Set cbText.TextStyle = adTextStyle(bTextStyleName, bFontName, cbFontWidth, cbFontHeight) 
SetElementXdata cbText, gAppName, msdXDatumTypeString, "Type:Bar" 
cbText.Level = cbLevel 
cbText.Color = 0 
'锁定并取消捕捉 
'cbText.IsLocked = True 
'cbText.IsSnappable = False 
cbModel.AddElement cbText 
cbText.Rewrite 
cbText.Redraw 
'网络线:竖直线###################### 
If bGrid Then 
cTmpGridVStart.X = cbLineStart.X 
cTmpGridVStart.Y = cTmpHtag.Y 
cTmpGridVEnd.X = cbLineStart.X 
cTmpGridVEnd.Y = cTmpHtag.Y + cbHeight - 0.3 * cbStep - cbFontHeight 
Set cTmpGridVLine = CreateLineElement2(Nothing, cTmpGridVStart, cTmpGridVEnd) 
cTmpGridVLine.Level = cbLevel 
cTmpGridVLine.Color = 64 
SetElementXdata cTmpGridVLine, gAppName, msdXDatumTypeString, "Type:Bar" 
'锁定并取消捕捉 
cTmpGridVLine.LineStyle = ActiveDesignFile.LineStyles("0") 
cTmpGridVLine.IsLocked = True 
cTmpGridVLine.IsSnappable = False 
cbModel.AddElement cTmpGridVLine 
cTmpGridVLine.Rewrite 
cTmpGridVLine.Redraw 
End If 
'网络线:竖直线###################### 
Next 
ProInfo = "桩号:" & cbModel.Name & " 比例:1:" & cbStep * 100 
ProInfoPosition.X = cbLeft + (cbWidth - Len(ProInfo) * 0.3 * cbStep) / 2 
ProInfoPosition.Y = cbBottom + cbHeight + 0.5 * cbStep '桩号放在上面 
'ProInfoPosition.Y = cbBottom - 0.1 * cbStep '桩号放在下面 
Set cbText = CreateTextElement1(Nothing, ProInfo, ProInfoPosition, Matrix3dIdentity) 
cbModel.AddElement cbText 
Set cbText.TextStyle = adTextStyle(bTextStyleName, bFontName, cbFontWidth, cbFontHeight) 
SetElementXdata cbText, gAppName, msdXDatumTypeString, "Type:Bar" 
cbText.TextStyle.Height = 0.4 * cbStep 
cbText.TextStyle.Width = 0.4 * cbStep 
cbText.Color = 0 
cbText.Level = cbLevel 
cbText.Rewrite 
cbText.Redraw 
End Sub 
'Sub TestTextStyle() 
'adTextStyle "覃东欧国家", "宋体", 10, 10 
'End Sub 
'查找并设置文本样式格式 
'参数:样式名、字体名、高度、宽度 
'若样式不存在则创建，若存在则设置 
'Function adTextStyle(txtStyleName As String, txtFontName As String, txtStyleHeight As Single, txtStyleWidth As Single) As TextStyle 
'Dim aTextStyle As TextStyle 
'Set aTextStyle = ActiveDesignFile.TextStyles.Find(txtStyleName) 
'If aTextStyle Is Nothing Then 
'Set aTextStyle = ActiveDesignFile.TextStyles.Add(Nothing, txtStyleName) 
'aTextStyle.Height = txtStyleHeight 
'aTextStyle.Width = txtStyleWidth 
'aTextStyle.Font = FindFontByName(txtFontName) 
''Else 
''aTextStyle.Height = txtStyleHeight 
''aTextStyle.Width = txtStyleWidth 
''aTextStyle.Font = FindFontByName(txtFontName) 
'End If 
'Set adTextStyle = aTextStyle 
'End Function 
Function FindFontByName(fFontName As String) As Font 
Dim fFonts As Fonts 
Dim fFont As Font 
Set FindFontByName = Nothing 
Set fFonts = ActiveDesignFile.Fonts 
For Each fFont In fFonts 
If fFont.Name = fFontName Then Set FindFontByName = fFont 
NextEnd Function 
'********************查找绘图范围(只处理直线对象)************** 
Sub ViewArea(vaModel As ModelReference) 
Dim m As ModelReference 
Dim Ee As ElementEnumerator 
Dim Ele As LineElement 
miniX = 1E+15 
miniY = 1E+15 
maxX = -1E+15 
maxY = -1E+15 
Set m = vaModel 
Set Ee = m.GraphicalElementCache.Scan 
Do While Ee.MoveNext 
If Ee.Current.Type = msdElementTypeLine Or Ee.Current.Type = msdElementTypeLineString Then 
If Ee.Current.Level.IsEffectivelyDisplayedInView(ActiveDesignFile.Views(1)) Then 
Set Ele = Ee.Current 
 MiniMax Ele 
End If 
End If 
Loop 
'ShowMessage miniX & " " & miniY & " " & maxX & " " & maxY 
End Sub 
Sub MiniMax(Ele As LineElement) 
Dim tEle As LineElement 
Dim vPoints() As Point3d 
Dim i As Long 
vPoints = Ele.GetVertices 
For i = LBound(vPoints) To UBound(vPoints) 
If vPoints(i).X < miniX Then miniX = vPoints(i).X 
If vPoints(i).Y < miniY Then miniY = vPoints(i).Y 
If vPoints(i).X > maxX Then maxX = vPoints(i).X 
If vPoints(i).Y > maxY Then maxY = vPoints(i).Y 
Next 
End Sub
```