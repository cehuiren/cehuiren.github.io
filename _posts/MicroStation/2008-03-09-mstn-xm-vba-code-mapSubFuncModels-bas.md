---
layout: post
title:  "MicroStation XM 断面处理程序源码-模块-mapSubFuncModels"
date: 2008-03-09
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA XM 代码 
excerpt: MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。
author: QinDong
---
* content
{:toc}

MicroStation XM VBA编写的工程测量断面处理程序，与目前的MicroStation CE不兼容，需稍作调整方可在CE中运行。代码中大部分代码和功能、过程与目前高版一样，可以参考使用。

>模块名：mapSubFuncModels.bas

```vb
Attribute VB_Name = "mapSubFuncModels"
'此模块是包含中间过程或函数

'Sub DrawPoint(dpNum As Long, dpX As Double, dpY As Double, dpH As Double)
'Dim elePoint As PointElement
'End Sub

Function ChkSideLen(p1X As Double, p1Y As Double, p2X As Double, p2Y As Double, p3X As Double, p3Y As Double, ln As Double) As Boolean
Dim s1 As Double
Dim s2 As Double
Dim s3 As Double
s1 = (p2X - p1X) * (p2X - p1X) + (p2Y - p1Y) * (p2Y - p1Y)
s2 = (p3X - p2X) * (p3X - p2X) + (p3Y - p2Y) * (p3Y - p2Y)
s3 = (p1X - p3X) * (p1X - p3X) + (p1Y - p3Y) * (p1Y - p3Y)
ChkSideLen = False
If s1 <= ln * ln And s2 <= ln * ln And s3 <= ln * ln Then ChkSideLen = True
End Function
'等高线B样条化封闭线处理
Sub SplineLevelLine_closed(cv As Single, cz As Integer, WaterEle As Double, MainLineColor As Integer, CountLineColor As Integer, WaterLineColor As Integer, E2D3D As String)
    Dim oBsplineCurve As New BsplineCurve
    Dim Ee As ElementEnumerator
    Dim LineEle As LineElement
    'Dim linePnts() As Point2d
    Dim CurLineEle As Double
    Dim LVertices() As Point3d
    Dim indx As Long
    Dim oBsplineCurveElement As BsplineCurveElement
    
Dim mLvlGrp As NamedGroupElement
Dim clvlGrp As NamedGroupElement

If ActiveModelReference.GetNamedGroup("mainlevelline") Is Nothing Then
Set mLvlGrp = ActiveModelReference.AddNewNamedGroup("mainlevelline")
Else
Set mLvlGrp = ActiveModelReference.GetNamedGroup("mainlevelline")
End If
If ActiveModelReference.GetNamedGroup("countlevelline") Is Nothing Then
Set clvlGrp = ActiveModelReference.AddNewNamedGroup("countlevelline")
Else
Set clvlGrp = ActiveModelReference.GetNamedGroup("countlevelline")
End If

    
    On Error Resume Next
    Set Ee = ActiveModelReference.GraphicalElementCache.Scan
    Do While Ee.MoveNext
        If Ee.Current.Type = 4 And (Not Ee.Current.HasXData("surmap_points")) And Trim(Ee.Current.Level.Name) = "三角网" Then
                Set LineEle = Ee.Current
                'linePnts = LineEle.GetVertices
            If LineEle.StartPoint.X = LineEle.EndPoint.X Then
            If LineEle.VerticesCount < 6 Then
            While LineEle.VerticesCount < 6
                If cz = 3 Then
                Set LineEle = InsertVertics(LineEle, cv)
                    LineEle.Rewrite
                End If
                
                If cz = 2 Or cz = 1 Then
                Set LineEle = InsertVerticsCenter(LineEle)
                    LineEle.Rewrite
                End If
            Wend
            Else
                If cz = 3 Then
                Set LineEle = InsertVertics(LineEle, cv)
                    LineEle.Rewrite
                End If
                
                If cz = 2 Then
                Set LineEle = InsertVerticsCenter(LineEle)
                    LineEle.Rewrite
                End If
            End If
              LVertices = LineEle.GetVertices
              If E2D3D = "2D" Then
              CurLineEle = LineEle.StartPoint.Z
              For indx = LBound(LVertices) To UBound(LVertices)
              LVertices(indx).Z = 0
              Next
              End If
              oBsplineCurve.SetPoles LVertices 'LineEle.GetVertices
              oBsplineCurve.Closed = True
              oBsplineCurve.Order = 6
              Set oBsplineCurveElement = CreateBsplineCurveElement1(Nothing, oBsplineCurve)
              If LineEle.StartPoint.Z > WaterEle Then
                If (LineEle.StartPoint.Z Mod (dgxDist * 5)) = 0 Then
                    oBsplineCurveElement.Level = FindLevel("等高线计曲线")
                    oBsplineCurveElement.Color = CountLineColor
                    oBsplineCurveElement.LineWeight = 1
                    oBsplineCurveElement.LineStyle = ActiveDesignFile.LineStyles("0")
                    SetElementXdata oBsplineCurveElement, "surmap_countlevelline", msdXDatumTypeString, CurLineEle
                Else
                    oBsplineCurveElement.Level = FindLevel("等高线首曲线")
                    oBsplineCurveElement.Color = MainLineColor
                    oBsplineCurveElement.LineWeight = 0
                    oBsplineCurveElement.LineStyle = ActiveDesignFile.LineStyles("0")
                    SetElementXdata oBsplineCurveElement, "surmap_mainlevelline", msdXDatumTypeString, CurLineEle
                End If
              Else
                If (LineEle.StartPoint.Z Mod (dgxDist * 5)) = 0 Then
                    oBsplineCurveElement.Level = FindLevel("等深线计曲线")
                    oBsplineCurveElement.Color = WaterLineColor
                    oBsplineCurveElement.LineWeight = 1
                    oBsplineCurveElement.LineStyle = ActiveDesignFile.LineStyles("3")
                    SetElementXdata oBsplineCurveElement, "surmap_countlevelline", msdXDatumTypeString, CurLineEle
                Else
                    oBsplineCurveElement.Level = FindLevel("等深线首曲线")
                    oBsplineCurveElement.Color = WaterLineColor
                    oBsplineCurveElement.LineWeight = 0
                    oBsplineCurveElement.LineStyle = ActiveDesignFile.LineStyles("3")
                    SetElementXdata oBsplineCurveElement, "surmap_mainlevelline", msdXDatumTypeString, CurLineEle
                End If
              End If
                ActiveModelReference.AddElement oBsplineCurveElement
                oBsplineCurveElement.Rewrite
                If oBsplineCurveElement.HasXData("surmap_countlevelline") Then
                clvlGrp.AddMember oBsplineCurveElement
                clvlGrp.Rewrite
                End If
                If oBsplineCurveElement.HasXData("surmap_mainlevelline") Then
                mLvlGrp.AddMember oBsplineCurveElement
                mLvlGrp.Rewrite
                End If
                oBsplineCurveElement.Redraw msdDrawingModeNormal
                ActiveModelReference.RemoveElement LineEle
            End If
        End If
    Loop

End Sub

'等高线B样条化开曲线处理
Sub SplineLevelLine_open(cv As Single, cz As Integer, WaterEle As Double, MainLineColor As Integer, CountLineColor As Integer, WaterLineColor As Integer, E2D3D As String)
    Dim oBsplineCurve As New BsplineCurve
    Dim Ee As ElementEnumerator
    Dim LineEle As LineElement
    'Dim linePnts() As Point2d
   
    Dim oBsplineCurveElement As BsplineCurveElement
    'Dim cntPoint(5) As Point3d
    'Dim Vertics() As Point3d
    Dim CurLineEle As Double
    Dim LVertices() As Point3d
    Dim indx As Long
    
Dim mLvlGrp As NamedGroupElement
Dim clvlGrp As NamedGroupElement

If ActiveModelReference.GetNamedGroup("mainlevelline") Is Nothing Then
Set mLvlGrp = ActiveModelReference.AddNewNamedGroup("mainlevelline")
Else
Set mLvlGrp = ActiveModelReference.GetNamedGroup("mainlevelline")
End If
If ActiveModelReference.GetNamedGroup("countlevelline") Is Nothing Then
Set clvlGrp = ActiveModelReference.AddNewNamedGroup("countlevelline")
Else
Set clvlGrp = ActiveModelReference.GetNamedGroup("countlevelline")
End If
    
    
    On Error Resume Next
    Set Ee = ActiveModelReference.GraphicalElementCache.Scan
    Do While Ee.MoveNext
        If (Ee.Current.Type = 4 Or Ee.Current.Type = 3) And (Not Ee.Current.HasXData("surmap_points")) And Trim(Ee.Current.Level.Name) = "三角网" Then
                Set LineEle = Ee.Current
                'linePnts = LineEle.GetVertices
            If LineEle.StartPoint.X <> LineEle.EndPoint.X Then
              
            If LineEle.VerticesCount < 6 Then
                While LineEle.VerticesCount < 6
                Set LineEle = InsertVerticsCenter(LineEle)
                    LineEle.Rewrite
                Wend
            Else
                If cz = 3 Then
                Set LineEle = InsertVertics(LineEle, cv)
                    LineEle.Rewrite
                End If
                
                If cz = 2 Then
                Set LineEle = InsertVerticsCenter(LineEle)
                    LineEle.Rewrite
                End If
             End If
              LVertices = LineEle.GetVertices
              If E2D3D = "2D" Then
              CurLineEle = LineEle.StartPoint.Z
              For indx = LBound(LVertices) To UBound(LVertices)
              LVertices(indx).Z = 0
              Next
              End If
              
              oBsplineCurve.SetPoles LVertices 'LineEle.GetVertices
              oBsplineCurve.Closed = False
              oBsplineCurve.Order = 6
                Set oBsplineCurveElement = CreateBsplineCurveElement1(Nothing, oBsplineCurve)
                'If E2D3D = "2D" Then SetElementXdata oBsplineCurveElement, "surmap_elevation", msdXDatumTypeString, CurLineEle
              If LineEle.StartPoint.Z > WaterEle Then
                If (LineEle.StartPoint.Z Mod (dgxDist * 5)) = 0 Then
                    oBsplineCurveElement.Level = FindLevel("等高线计曲线")
                    oBsplineCurveElement.Color = CountLineColor
                    oBsplineCurveElement.LineWeight = 1
                    oBsplineCurveElement.LineStyle = ActiveDesignFile.LineStyles("0")
                    SetElementXdata oBsplineCurveElement, "surmap_countlevelline", msdXDatumTypeString, CurLineEle
                Else
                    oBsplineCurveElement.Level = FindLevel("等高线首曲线")
                    oBsplineCurveElement.Color = MainLineColor
                    oBsplineCurveElement.LineWeight = 0
                    oBsplineCurveElement.LineStyle = ActiveDesignFile.LineStyles("0")
                    SetElementXdata oBsplineCurveElement, "surmap_mainlevelline", msdXDatumTypeString, CurLineEle
                End If
              Else
                If (LineEle.StartPoint.Z Mod (dgxDist * 5)) = 0 Then
                    oBsplineCurveElement.Level = FindLevel("等深线计曲线")
                    oBsplineCurveElement.Color = WaterLineColor
                    oBsplineCurveElement.LineWeight = 1
                    oBsplineCurveElement.LineStyle = ActiveDesignFile.LineStyles("3")
                    SetElementXdata oBsplineCurveElement, "surmap_countlevelline", msdXDatumTypeString, CurLineEle
                Else
                    oBsplineCurveElement.Level = FindLevel("等深线首曲线")
                    oBsplineCurveElement.Color = WaterLineColor
                    oBsplineCurveElement.LineWeight = 0
                    oBsplineCurveElement.LineStyle = ActiveDesignFile.LineStyles("3")
                    SetElementXdata oBsplineCurveElement, "surmap_mainlevelline", msdXDatumTypeString, CurLineEle
                End If
              End If
                ActiveModelReference.AddElement oBsplineCurveElement
                oBsplineCurveElement.Rewrite
                If oBsplineCurveElement.HasXData("surmap_countlevelline") Then
                clvlGrp.AddMember oBsplineCurveElement
                clvlGrp.Rewrite
                End If
                If oBsplineCurveElement.HasXData("surmap_mainlevelline") Then
                mLvlGrp.AddMember oBsplineCurveElement
                mLvlGrp.Rewrite
                End If
                oBsplineCurveElement.Redraw msdDrawingModeNormal
                ActiveModelReference.RemoveElement LineEle
            End If
        End If
    Loop

End Sub

'查找并设置文本样式格式
'参数:样式名、字体名、高度、宽度
'若样式不存在则创建，若存在则设置
Function adTextStyle(txtStyleName As String, txtFontName As String, txtStyleHeight As Single, txtStyleWidth As Single) As TextStyle
Dim aTextStyle As TextStyle
Set aTextStyle = ActiveDesignFile.TextStyles.Find(txtStyleName)
If aTextStyle Is Nothing Then
Set aTextStyle = ActiveDesignFile.TextStyles.Add(Nothing, txtStyleName)
aTextStyle.Height = txtStyleHeight
aTextStyle.Width = txtStyleWidth
aTextStyle.Font = FindFontByName(txtFontName)
'Else
'aTextStyle.Height = txtStyleHeight
'aTextStyle.Width = txtStyleWidth
'aTextStyle.Font = FindFontByName(txtFontName)
End If
Set adTextStyle = aTextStyle
End Function


Function FindFontByName(fFontName As String) As Font
Dim fFonts As Fonts
Dim fFont As Font
Set FindFontByName = Nothing
Set fFonts = ActiveDesignFile.Fonts
For Each fFont In fFonts
If fFont.Name = fFontName Then Set FindFontByName = fFont
Next
End Function

Sub LoadPoints(DtaFileName As String, fileFormat As String, zj As String, pXs As Integer, pntColor As Integer, rmkColor As Integer)
Dim PointsFileName As String '地形碎部点文件名
Dim pntLevel As Level
Dim FileNumber As Integer
Dim pX As Double
Dim pY As Double
Dim pH As Double
Dim pNum As String
Dim pNumTmp As Long
Dim pInfo As String
Dim InfoStr As String
Dim ps As String
Dim PntEleId As String
Dim LblEleId As String

'读取数据文件一行
Dim DataLine As String

'展点标注
Dim cbText As TextElement
Dim cbTextPos As Point3d
'标注小数位数
Dim cbPoints As Integer

'小平面法生成三角
Dim xpmPoints(1) As Point3d
Dim xpmLine As LineElement
On Error GoTo ext
Dim dxGroup As NamedGroupElement
Dim dxPn As NamedGroupElement
Dim dxLvl As NamedGroupElement
Dim dxPointsCount As Long
Dim DtaLine As String

dxPointsCount = 0

Dim fw As String
Dim fw2
If ActiveModelReference.GetNamedGroup("dxpoints") Is Nothing Then
Set dxGroup = ActiveModelReference.AddNewNamedGroup("dxpoints")
Else
Set dxGroup = ActiveModelReference.GetNamedGroup("dxpoints")
End If
If ActiveModelReference.GetNamedGroup("dxpointname") Is Nothing Then
Set dxPn = ActiveModelReference.AddNewNamedGroup("dxpointName")
Else
Set dxPn = ActiveModelReference.GetNamedGroup("dxpointName")
End If
If ActiveModelReference.GetNamedGroup("dxpointElevation") Is Nothing Then
Set dxLvl = ActiveModelReference.AddNewNamedGroup("dxpointElevation")
Else
Set dxLvl = ActiveModelReference.GetNamedGroup("dxpointElevation")
End If

If ActiveModelReference.HasXData("surmap_fw") Then
    fw = getModelXdata(ActiveModelReference, "surmap_fw", msdXDatumTypeString)
    If Trim(fw) <> "" Then
    fw2 = Split(fw, ",")
        If UBound(fw2) > 1 Then
            miniN = fw2(0)
            miniE = fw2(1)
            miniH = fw2(2)
            
            maxN = fw2(3)
            maxE = fw2(4)
            maxH = fw2(5)
        End If
    Else
    maxN = 0
    maxE = 0
    maxH = 0
    miniN = 1E+20
    miniE = 1E+20
    miniH = 1E+20
    End If
Else
    maxN = 0
    maxE = 0
    maxH = 0
    miniN = 1E+20
    miniE = 1E+20
    miniH = 1E+20
End If



tPoints = 1

cbPoints = pXs

PointsFileName = DtaFileName
FileNumber = FreeFile
pNumTmp = 0
If Trim(PointsFileName) <> "" Then
    Open PointsFileName For Input As #FileNumber
    While Not EOF(FileNumber)
       Select Case fileFormat
       Case "psenh"
        pNumTmp = pNumTmp + 1
        Line Input #FileNumber, DataLine
        If Trim(DataLine) <> "" Then
            pNum = Trim(readData(DataLine, 1, 5))
            pInfo = Trim(readData(DataLine, 2, 5))
            pX = Trim(readData(DataLine, 3, 5))
            pY = Trim(readData(DataLine, 4, 5))
            pH = Trim(readData(DataLine, 5, 5))
        End If
       Case "psneh"
        pNumTmp = pNumTmp + 1
        Line Input #FileNumber, DataLine
        If Trim(DataLine) <> "" Then
            pNum = readData(DataLine, 1, 5)
            pInfo = Trim(readData(DataLine, 2, 5))
            pX = readData(DataLine, 4, 5)
            pY = readData(DataLine, 3, 5)
            pH = readData(DataLine, 5, 5)
        End If
       Case "penh"
        pNumTmp = pNumTmp + 1
        Line Input #FileNumber, DataLine
        If Trim(DataLine) <> "" Then
            pNum = readData(DataLine, 1, 4)
            pX = readData(DataLine, 2, 4)
            pY = readData(DataLine, 3, 4)
            pH = readData(DataLine, 4, 4)
        End If
       Case "pneh"
        pNumTmp = pNumTmp + 1
        Line Input #FileNumber, DataLine
        If Trim(DataLine) <> "" Then
            pNum = readData(DataLine, 1, 4)
            pX = readData(DataLine, 3, 4)
            pY = readData(DataLine, 2, 4)
            pH = readData(DataLine, 4, 4)
        End If
       Case "enh"
        pNumTmp = pNumTmp + 1
        pNum = Format(pNumTmp, "0")
        Line Input #FileNumber, DataLine
        If Trim(DataLine) <> "" Then
            pX = readData(DataLine, 1, 3)
            pY = readData(DataLine, 2, 3)
            pH = readData(DataLine, 3, 3)
        End If
       Case "neh"
        pNumTmp = pNumTmp + 1
        pNum = Format(pNumTmp, "0")
        Line Input #FileNumber, DataLine
        If Trim(DataLine) <> "" Then
            pX = readData(DataLine, 2, 3)
            pY = readData(DataLine, 1, 3)
            pH = readData(DataLine, 3, 3)
        End If
      End Select
      
If pX <> -666.666 And pY <> -666.666 And pH <> -666.666 Then
        '取得地形图范围
        If pX > maxE Then maxE = pX
        If pY > maxN Then maxN = pY
        If pH > maxH Then maxH = pH
        
        If pX < miniE Then miniE = pX
        If pY < miniN Then miniN = pY
        If pH < miniH Then miniH = pH
        
        'SetModelXdata ActiveModelReference, "surmap_fw", msdXDatumTypeString, Format(miniN, "0.0######") & "," & Format(miniE, "0.0######") & "," & Format(miniH, "0.0######") & "," & Format(maxN, "0.0######") & "," & Format(maxE, "0.0######") & "," & Format(maxH, "0.0######")
        '展点小平面法生成三角
        xpmPoints(0).X = pX
        xpmPoints(0).Y = pY
        xpmPoints(0).Z = pH
        xpmPoints(1).X = pX
        xpmPoints(1).Y = pY
        xpmPoints(1).Z = pH
        
        Set xpmLine = CreateLineElement1(Nothing, xpmPoints)
        xpmLine.Level = FindLevel("地形点标记")
        xpmLine.Color = pntColor
        'xpmLine.LineWeight = 1
        ActiveModelReference.AddElement xpmLine
        PntEleId = DLongToString(ActiveModelReference.GraphicalElementCache.GetLastValidElement.id)
'        InfoStr = pNum & "|" & pNumTmp & "|" & pInfo & "|" & PntEleId
'        SetElementXdata xpmLine, "surmap_points", msdXDatumTypeString, InfoStr '"pnt"
'        xpmLine.Rewrite
'        xpmLine.Redraw
'        dxGroup.AddMember xpmLine

        cbTextPos.X = xpmPoints(0).X + 2 * curScale
        cbTextPos.Y = xpmPoints(0).Y + 2 * curScale
        cbTextPos.X = xpmPoints(0).X
    
        If zj = "dh" Then
            Set cbText = CreateTextElement1(Nothing, pNum, cbTextPos, Matrix3dIdentity)
        Else
            Set cbText = CreateTextElement1(Nothing, Format(xpmPoints(0).Z, "0." & String(cbPoints, "0")), cbTextPos, Matrix3dIdentity)
        End If
        ActiveModelReference.AddElement cbText
        LblEleId = DLongToString(ActiveModelReference.GraphicalElementCache.GetLastValidElement.id)
        SetElementXdata cbText, "surmap_pointlabel", msdXDatumTypeString, pNumTmp & "|" & PntEleId
        Set cbText.TextStyle = adTextStyle("地形点标注", "WORKING", 0.4, 0.4)
        cbText.TextStyle.Height = 2 * curScale
        cbText.TextStyle.Width = 2 * curScale
        cbText.Level = FindLevel("地形点标注")
        cbText.TextStyle.Color = rmkColor
        cbText.Rewrite
        cbText.Redraw
        
        InfoStr = pNum & "|" & pNumTmp & "|" & pInfo & "|" & LblEleId
        SetElementXdata xpmLine, "surmap_points", msdXDatumTypeString, InfoStr '"pnt"
        xpmLine.Rewrite
        xpmLine.Redraw
        dxGroup.AddMember xpmLine
        
    If zj = "dh" Then
        dxPn.AddMember cbText
    Else
        dxLvl.AddMember cbText
    End If
    If dxPointsCount = 500 Then
        dxGroup.Rewrite
        dxPointsCount = 0
        If zj = "dh" Then
        dxPn.Rewrite
        Else
        dxLvl.Rewrite
        End If
    End If
    dxPointsCount = dxPointsCount + 1
    tPoints = tPoints + 1
End If
    Wend
        '设计地形范围
        SetModelXdata ActiveModelReference, "surmap_fw", msdXDatumTypeString, Format(miniN, "0.0######") & "," & Format(miniE, "0.0######") & "," & Format(miniH, "0.0######") & "," & Format(maxN, "0.0######") & "," & Format(maxE, "0.0######") & "," & Format(maxH, "0.0######")
dxGroup.Rewrite
If zj = "dh" Then
dxPn.Rewrite
Else
dxLvl.Rewrite
End If
    Close #FileNumber
Else
    MsgBox "请选择地形数据文件!"
End If
ext:
End Sub

'绘三角网
Sub DrawTriangles(selStyle As Integer)
    Dim StartPoint As Point3d
    Dim Point As Point3d, point2 As Point3d
    Dim lngTemp As Long
    Dim triEe As ElementEnumerator
    Dim triEe2 As ElementEnumerator
    Dim triModel As ModelReference
    Dim tmpLine As LineElement
    
    Dim LastEleId As Long
    Dim triId As Long
    Dim sEle As Element
    Dim dxGrp As NamedGroupElement
    Dim oScanCriteria As ElementScanCriteria
    
    Set oScanCriteria = New ElementScanCriteria
    Dim dtmGrp As NamedGroupElement
'miniE = 1E+20
'miniN = 1E+20
'miniH = 1E+20

Dim fw As String
Dim fw2
If ActiveModelReference.GetNamedGroup("dtm") Is Nothing Then
Set dtmGrp = ActiveModelReference.AddNewNamedGroup("dtm")
Else
Set dtmGrp = ActiveModelReference.GetNamedGroup("dtm")
End If

If ActiveModelReference.HasXData("surmap_fw") Then
    fw = getModelXdata(ActiveModelReference, "surmap_fw", msdXDatumTypeString)
    If Trim(fw) <> "" Then
    fw2 = Split(fw, ",")
        If UBound(fw2) > 1 Then
            miniN = fw2(0)
            miniE = fw2(1)
            miniH = fw2(2)
            
            maxN = fw2(3)
            maxE = fw2(4)
            maxH = fw2(5)
        End If
    Else
    maxN = 0
    maxE = 0
    maxH = 0
    miniN = 1E+20
    miniE = 1E+20
    miniH = 1E+20
    End If
Else
    maxN = 0
    maxE = 0
    maxH = 0
    miniN = 1E+20
    miniE = 1E+20
    miniH = 1E+20
End If


     LastEleId = ActiveModelReference.GraphicalElementCache.GetLastValidElement.CacheIndex


    Set triModel = ActiveModelReference
If selStyle = 1 Then
'自动选择全部地形点
If ActiveModelReference.GetNamedGroup("dxpoints") Is Nothing Then
    oScanCriteria.ExcludeAllLevels
    oScanCriteria.IncludeLevel FindLevel("地形点标记")
    Set triEe = triModel.GraphicalElementCache.Scan(oScanCriteria)
    Do While triEe.MoveNext
       If triEe.Current.Type = msdElementTypeLine And triEe.Current.HasXData("surmap_points") Then
            Set tmpLine = triEe.Current
            If tmpLine.StartPoint.X > maxE Then maxE = tmpLine.StartPoint.X
            If tmpLine.StartPoint.Y > maxN Then maxN = tmpLine.StartPoint.Y
            If tmpLine.StartPoint.Z > maxH Then maxH = tmpLine.StartPoint.Z

            If tmpLine.StartPoint.X < miniE Then miniE = tmpLine.StartPoint.X
            If tmpLine.StartPoint.Y < miniN Then miniN = tmpLine.StartPoint.Y
            If tmpLine.StartPoint.Z < miniH Then miniH = tmpLine.StartPoint.Z
       triModel.SelectElement triEe.Current
       End If
    Loop
Else
    Set dxGrp = ActiveModelReference.GetNamedGroup("dxpoints")
    dxGrp.SelectElements msdMemberTraverseSimple
End If

Else
'手动选择部分地形点
    Set triEe = triModel.GetSelectedElements
    Do While triEe.MoveNext
       If triEe.Current.HasXData("surmap_points") Then
            Set tmpLine = triEe.Current
            If tmpLine.StartPoint.X > maxE Then maxE = tmpLine.StartPoint.X
            If tmpLine.StartPoint.Y > maxN Then maxN = tmpLine.StartPoint.Y
            If tmpLine.StartPoint.Z > maxH Then maxH = tmpLine.StartPoint.Z

            If tmpLine.StartPoint.X < miniE Then miniE = tmpLine.StartPoint.X
            If tmpLine.StartPoint.Y < miniN Then miniN = tmpLine.StartPoint.Y
            If tmpLine.StartPoint.Z < miniH Then miniH = tmpLine.StartPoint.Z
       Else
       triModel.UnselectElement triEe.Current
       End If
    Loop


End If
    SetModelXdata ActiveModelReference, "surmap_fw", msdXDatumTypeString, Format(miniN, "0.0######") & "," & Format(miniE, "0.0######") & "," & Format(miniH, "0.0######") & "," & Format(maxN, "0.0######") & "," & Format(maxE, "0.0######") & "," & Format(maxH, "0.0######")
    CadInputQueue.SendCommand "mdl load facet", False
    CadInputQueue.SendCommand "FACET TRIANGULATE XYPOINTS"

'   坐标以主单位计
    StartPoint.X = 0
    StartPoint.Y = 0
    StartPoint.Z = 0#

'   给当前命令传送一个数据点
    Point.X = StartPoint.X
    Point.Y = StartPoint.Y
    Point.Z = StartPoint.Z
    CadInputQueue.SendDataPoint Point, 1

    triId = ActiveModelReference.GraphicalElementCache.GetLastValidElement.CacheIndex
    If LastEleId <> triId Then
        Set sEle = ActiveModelReference.GraphicalElementCache.GetLastValidElement
        sEle.Level = FindLevel("三角网")
        sEle.Color = 9
        sEle.Rewrite
        sEle.Redraw
        dtmGrp.AddMember sEle
        dtmGrp.Rewrite
    End If

End Sub

'创建断面桩号标注文本
Sub CrtStageText(str As String, midpoint As Point3d, rPoint As Point3d)
Dim txtTmp As TextElement
Dim txtPoint As Point3d
Dim tSize As Double
Dim rAng As Double
tSize = 2 * curScale 'onPaperSize * curScale
rAng = Azimuth(midpoint.X, midpoint.Y, rPoint.X, rPoint.Y)
If rAng < 0 Then
txtPoint.X = midpoint.X + tSize * Cos(rAng)
txtPoint.Y = midpoint.Y + tSize * Sin(rAng)
ElseIf rAng > 0 Then
txtPoint.X = midpoint.X - Len(str) * tSize * Cos(rAng) / 2 + tSize * Cos(rAng)
txtPoint.Y = midpoint.Y - Len(str) * tSize * Sin(rAng) / 2 + tSize * Sin(rAng)
Else
txtPoint.X = midpoint.X - Len(str) * tSize * Cos(rAng) / 2 + tSize * Cos(rAng)
txtPoint.Y = midpoint.Y - Len(str) * tSize * Sin(rAng) / 2 + tSize * Sin(rAng)
End If

Set txtTmp = CreateTextElement1(Nothing, str, txtPoint, Matrix3dFromAxisAndRotationAngle(2, rAng))
txtTmp.TextStyle.Height = tSize
txtTmp.TextStyle.Width = tSize
txtTmp.Color = curColor
txtTmp.Level = FindLevel("断面布置线")
ActiveModelReference.AddElement txtTmp
txtTmp.Rewrite
txtTmp.Redraw
End Sub

Function InsertVertics(Ele As LineElement, cvp As Single) As LineElement
Dim vCnt As Long
Dim indx As Long
Dim eleVertics() As Point3d
Dim insertPoints() As Point3d
Dim LineEle As LineElement
                Set LineEle = Ele
                vCnt = LineEle.VerticesCount
                eleVertics = LineEle.GetVertices

                ReDim insertPoints(vCnt - 1)
                For indx = 0 To vCnt - 2
                insertPoints(indx).X = eleVertics(indx).X + (eleVertics(indx + 1).X - eleVertics(indx).X) * cvp
                insertPoints(indx).Y = eleVertics(indx).Y + (eleVertics(indx + 1).Y - eleVertics(indx).Y) * cvp
                insertPoints(indx).Z = eleVertics(indx).Z + (eleVertics(indx + 1).Z - eleVertics(indx).Z) * cvp
                LineEle.InsertVertex 3 * indx + 1, insertPoints(indx)
                
                insertPoints(indx).X = eleVertics(indx).X + (eleVertics(indx + 1).X - eleVertics(indx).X) * (1 - cvp)
                insertPoints(indx).Y = eleVertics(indx).Y + (eleVertics(indx + 1).Y - eleVertics(indx).Y) * (1 - cvp)
                insertPoints(indx).Z = eleVertics(indx).Z + (eleVertics(indx + 1).Z - eleVertics(indx).Z) * (1 - cvp)
                LineEle.InsertVertex 3 * indx + 2, insertPoints(indx)
                
                
                Next
'                LineEle.Redraw msdDrawingModeNormal
'                LineEle.Rewrite
Set InsertVertics = LineEle
End Function


Function InsertVerticsCenter(Ele As LineElement) As LineElement
Dim vCnt As Long
Dim indx As Long
Dim eleVertics() As Point3d
Dim insertPoints() As Point3d
Dim LineEle As LineElement
                Set LineEle = Ele
                vCnt = LineEle.VerticesCount
                eleVertics = LineEle.GetVertices

                ReDim insertPoints(vCnt - 1)
                For indx = 0 To vCnt - 2
                insertPoints(indx).X = eleVertics(indx).X + (eleVertics(indx + 1).X - eleVertics(indx).X) * 0.5
                insertPoints(indx).Y = eleVertics(indx).Y + (eleVertics(indx + 1).Y - eleVertics(indx).Y) * 0.5
                insertPoints(indx).Z = eleVertics(indx).Z + (eleVertics(indx + 1).Z - eleVertics(indx).Z) * 0.5
                LineEle.InsertVertex 2 * indx + 1, insertPoints(indx)
                
'                insertPoints(indx).X = eleVertics(indx).X + (eleVertics(indx + 1).X - eleVertics(indx).X) * (1 - cvp)
'                insertPoints(indx).Y = eleVertics(indx).Y + (eleVertics(indx + 1).Y - eleVertics(indx).Y) * (1 - cvp)
'                insertPoints(indx).Z = eleVertics(indx).Z + (eleVertics(indx + 1).Z - eleVertics(indx).Z) * (1 - cvp)
'                LineEle.InsertVertex 3 * indx + 2, insertPoints(indx)
                
                
                Next
'                LineEle.Redraw msdDrawingModeNormal
'                LineEle.Rewrite
Set InsertVerticsCenter = LineEle
End Function



'读取数据文件某一行指定列的数据指定参数
Function readData(dta As String, pos As Integer, FieldCount As Integer) As String
Dim rpara
Dim Status As String
Dim DataTmp As String
Dim dlimiter As String
    dlimiter = ""
    DataTmp = dta
    If InStr(DataTmp, ",") <> 0 Then
        DataTmp = Replace(DataTmp, " ", "")
        dlimiter = ""
        rpara = Split(DataTmp, ",")
    ElseIf InStr(DataTmp, " ") <> 0 Then
        rpara = Split(DataTmp, " ")
        dlimiter = " "
    ElseIf InStr(DataTmp, vbTab) <> 0 Then
        rpara = Split(DataTmp, vbTab)
        dlimiter = vbTab
    Else
        Status = "error"
    End If

If Status <> "error" Then
        If pos - 1 <= UBound(rpara) And pos - 1 >= 0 And Trim(rpara(pos - 1)) <> "" Then
            readData = rpara(pos - 1)
        Else
            readData = "-666.666"
        End If
Else
    readData = "-666.666"
End If
End Function

Sub ChangeShape(ele1 As ShapeElement, ele2 As ShapeElement)
Dim Vcnt1 As Integer
Dim Vcnt2 As Integer
Dim Vers1() As Point3d
Dim Vers2() As Point3d
Dim Spoints(3) As Point3d
Dim Dpoints(3) As Point3d
'Dim Dpoints2(3) As Point3d
Dim i_index As Integer
Dim j_index As Integer
Dim newShpVers(3) As Point3d
Dim k As Integer
Dim sm As Boolean
Dim dp1 As Point3d
Dim dp2 As Point3d
Dim NewTri As ShapeElement
Dim triGrp As NamedGroupElement
Set triGrp = ActiveModelReference.GetNamedGroup("triangles")

Vcnt1 = ele1.VerticesCount
Vcnt2 = ele2.VerticesCount
Vers1 = ele1.GetVertices
Vers2 = ele2.GetVertices

For i_index = 0 To Vcnt1 - 2
 Spoints(i_index) = Point3dFromXYZ(0, 0, 0)
For j_index = 0 To Vcnt2 - 2
If Vers1(i_index).X = Vers2(j_index).X And (Vers2(j_index).Z <> -666666.666) And (Vers1(i_index).Z <> -666666.666) Then
 Spoints(i_index) = Vers1(i_index)
 End If
Next
Next

For i_index = 0 To UBound(Spoints) - 1
For j_index = 0 To UBound(Vers1) - 1
If Vers1(j_index).X = Spoints(i_index).X Then Vers1(j_index) = Point3dFromXYZ(0, 0, 0)
Next
Next

For i_index = 0 To UBound(Spoints) - 1
For j_index = 0 To UBound(Vers2) - 1
If Vers2(j_index).X = Spoints(i_index).X Then Vers2(j_index) = Point3dFromXYZ(0, 0, 0)
Next
Next



For i_index = 0 To UBound(Vers1) - 1
If Vers1(i_index).X <> 0 Then dp1 = Vers1(i_index)
Next

For i_index = 0 To UBound(Vers2) - 1
If Vers2(i_index).X <> 0 Then dp2 = Vers2(i_index)
Next

For i_index = 0 To UBound(Spoints)

If Spoints(i_index).X <> 0 Then
k = 0
newShpVers(k) = Spoints(i_index)
newShpVers(k + 1) = dp1
newShpVers(k + 2) = dp2
newShpVers(k + 3) = Spoints(i_index)

Set NewTri = CreateShapeElement1(Nothing, newShpVers)
NewTri.Level = FindLevel("三角网")
NewTri.Color = 4
NewTri.LineStyle = ActiveDesignFile.LineStyles("0")
ActiveModelReference.AddElement NewTri
NewTri.Rewrite
NewTri.Redraw msdDrawingModeNormal
    If Not (triGrp Is Nothing) Then
    If triGrp.GetMember(NewTri) Is Nothing Then triGrp.AddMember NewTri
    End If
End If
Next
    If Not (triGrp Is Nothing) Then
    triGrp.Rewrite
    End If
ActiveModelReference.RemoveElement ele1
ActiveModelReference.RemoveElement ele2

End Sub


Function DelLineZ(Ele As LineElement) As LineElement
Dim vCnt As Integer
Dim indx As Long
Dim LineEle  As LineElement
Set LineEle = Ele
                vCnt = LineEle.VerticesCount
                For indx = 1 To vCnt - 1
                LineEle.Vertex(indx).Z = 0
                LineEle.Rewrite
                Next
                
Set DelLineZ = LineEle
End Function


Sub ExportPoints(DtaFileName As String, fileFormat As String, SelMethod As Integer)
Dim PointsFileName As String '地形碎部点文件名
Dim FileNumber As Integer
Dim pX As Double
Dim pY As Double
Dim pH As Double
Dim pNum As String
Dim pNumTmp As Long
Dim ps As String
Dim DataLine As String
On Error GoTo ext
Dim dxGrp As NamedGroupElement
Dim tmpLine As LineElement
Dim triModel As ModelReference
Dim oScanCriteria As ElementScanCriteria

FileNumber = FreeFile
    Set triModel = ActiveModelReference
    Open DtaFileName For Output As #FileNumber
If SelMethod = 1 Then
'自动选择全部地形点
If ActiveModelReference.GetNamedGroup("dxpoints") Is Nothing Then
    oScanCriteria.ExcludeAllLevels
    oScanCriteria.IncludeLevel FindLevel("地形点标记")
    Set triEe = triModel.GraphicalElementCache.Scan(oScanCriteria)
    Do While triEe.MoveNext
       If triEe.Current.Type = msdElementTypeLine And triEe.Current.HasXData("surmap_points") Then
            Set tmpLine = triEe.Current
            pNumTmp = pNumTmp + 1
            pNum = pNumTmp
            pX = tmpLine.StartPoint.X
            pY = tmpLine.StartPoint.Y
            pH = tmpLine.StartPoint.Z
       Select Case fileFormat
       Case "psenh"
            DataLine = pNum & ",," & pX & "," & pY & "," & pH
       Case "psneh"
            DataLine = pNum & ",," & pY & "," & pX & "," & pH
       Case "penh"
            DataLine = pNum & "," & pX & "," & pY & "," & pH
       Case "pneh"
            DataLine = pNum & "," & pY & "," & pX & "," & pH
       Case "enh"
            DataLine = pX & "," & pY & "," & pH
       Case "neh"
            DataLine = pY & "," & pX & "," & pH
      End Select
            Print #FileNumber, DataLine
       End If
    Loop
Else
    triModel.UnselectAllElements
    Set dxGrp = ActiveModelReference.GetNamedGroup("dxpoints")
    dxGrp.SelectElements msdMemberTraverseSimple
    Set triEe = triModel.GetSelectedElements
    Do While triEe.MoveNext
       If triEe.Current.HasXData("surmap_points") And triEe.Current.Type = msdElementTypeLine Then
            Set tmpLine = triEe.Current
            pNumTmp = pNumTmp + 1
            pNum = pNumTmp
            pX = tmpLine.StartPoint.X
            pY = tmpLine.StartPoint.Y
            pH = tmpLine.StartPoint.Z
       Select Case fileFormat
       Case "psenh"
            DataLine = pNum & ",," & pX & "," & pY & "," & pH
       Case "psneh"
            DataLine = pNum & ",," & pY & "," & pX & "," & pH
       Case "penh"
            DataLine = pNum & "," & pX & "," & pY & "," & pH
       Case "pneh"
            DataLine = pNum & "," & pY & "," & pX & "," & pH
       Case "enh"
            DataLine = pX & "," & pY & "," & pH
       Case "neh"
            DataLine = pY & "," & pX & "," & pH
      End Select
            Print #FileNumber, DataLine
       End If
    Loop
    triModel.UnselectAllElements
End If

Else
'手动选择部分地形点
    Set triEe = triModel.GetSelectedElements
    Do While triEe.MoveNext
       If triEe.Current.HasXData("surmap_points") And triEe.Current.Type = msdElementTypeLine Then
            Set tmpLine = triEe.Current
            pNumTmp = pNumTmp + 1
            pNum = pNumTmp
            pX = tmpLine.StartPoint.X
            pY = tmpLine.StartPoint.Y
            pH = tmpLine.StartPoint.Z
       Select Case fileFormat
       Case "psenh"
            DataLine = pNum & ",," & pY & "," & pX & "," & pH
       Case "psneh"
            DataLine = pNum & ",," & pX & "," & pY & "," & pH
       Case "penh"
            DataLine = pNum & "," & pY & "," & pX & "," & pH
       Case "pneh"
            DataLine = pNum & "," & pX & "," & pY & "," & pH
       Case "enh"
            DataLine = pY & "," & pX & "," & pH
       Case "neh"
            DataLine = pX & "," & pY & "," & pH
      End Select
            Print #FileNumber, DataLine
       End If
    Loop
    triModel.UnselectAllElements
End If
Close #FileNumber
ext:
End Sub


Sub DelLevelLine()
    Dim oScanCriteria As ElementScanCriteria
    Set oScanCriteria = New ElementScanCriteria
    Dim lvlEe As ElementEnumerator
    Dim mLvlGrp As NamedGroupElement
    Dim clvlGrp As NamedGroupElement
    
If ActiveModelReference.GetNamedGroup("mainlevelline") Is Nothing Then
    oScanCriteria.ExcludeAllLevels
    oScanCriteria.IncludeLevel FindLevel("等高线首曲线")
    Set lvlEe = ActiveModelReference.GraphicalElementCache.Scan(oScanCriteria)
    Do While lvlEe.MoveNext
       If lvlEe.Current.Type = msdElementTypeBsplineCurve Then
       ActiveModelReference.RemoveElement lvlEe.Current
       End If
    Loop
Else
    ActiveModelReference.UnselectAllElements
    Set mLvlGrp = ActiveModelReference.GetNamedGroup("mainlevelline")
    mLvlGrp.SelectElements msdMemberTraverseSimple
    CadInputQueue.SendCommand "DELETE ELEMENT"
End If
    
If ActiveModelReference.GetNamedGroup("countlevelline") Is Nothing Then
    oScanCriteria.ExcludeAllLevels
    oScanCriteria.IncludeLevel FindLevel("等高线计曲线")
    Set lvlEe = ActiveModelReference.GraphicalElementCache.Scan(oScanCriteria)
    Do While lvlEe.MoveNext
       If lvlEe.Current.Type = msdElementTypeBsplineCurve Then
       ActiveModelReference.RemoveElement lvlEe.Current
       End If
    Loop
Else
    ActiveModelReference.UnselectAllElements
    Set mLvlGrp = ActiveModelReference.GetNamedGroup("countlevelline")
    mLvlGrp.SelectElements msdMemberTraverseSimple
    CadInputQueue.SendCommand "DELETE ELEMENT"
End If

End Sub

Sub LableLevelLine(lEle As LineElement)
    Dim oScanCriteria As ElementScanCriteria
    Dim lvlEe As ElementEnumerator
    Dim bspLine As BsplineCurveElement
    Dim interPoints1() As Point3d
    Dim interPoints2() As Point3d
    Dim interPointsM() As Point3d
    Dim lblI As Long
    Dim CurElevation As String
    Dim NewEle1 As Element
    Dim NewEle2 As Element
    Dim oeColor As Integer
    
    Dim Lpoints(1) As Point3d
    Dim cLine1 As LineElement
    Dim cLine2 As LineElement
    Dim lblWidth As Single
    Dim clvlGrp As NamedGroupElement
    Dim etxtEle As TextElement
    Dim lblScale As String
    Dim cScale As Single
    On Error Resume Next
    '提取绘图比例
    lblScale = LoadScalePara(ActiveDesignFile.DefaultModelReference, "(C)SurMap.com Parameter")
    If lblScale = "" Then
    cScale = 500 / 1000
    Else
    cScale = lblScale / 1000
    End If
    
    If ActiveModelReference.GetNamedGroup("countlevelline") Is Nothing Then
    Set clvlGrp = ActiveModelReference.AddNewNamedGroup("countlevelline")
    Else
    Set clvlGrp = ActiveModelReference.GetNamedGroup("countlevelline")
    End If

    
    Set oScanCriteria = New ElementScanCriteria
    oScanCriteria.ExcludeAllLevels
    oScanCriteria.IncludeLevel FindLevel("等高线计曲线")
    Set lvlEe = ActiveModelReference.GraphicalElementCache.Scan(oScanCriteria)
    Do While lvlEe.MoveNext
       If lvlEe.Current.Type = msdElementTypeBsplineCurve Then
       Set bspLine = lvlEe.Current
       If bspLine.HasXData("surmap_countlevelline") Then
       CurElevation = getElementXdata(bspLine, "surmap_countlevelline", msdXDatumTypeString)
       ElseIf bspLine.StartPoint.Z <> 0 Then
       CurElevation = bspLine.StartPoint.Z
       Else
       CurElevation = 0
       End If
    
    lblWidth = cScale * Len(CurElevation)
    Lpoints(0).X = lEle.StartPoint.X + lblWidth * Cos(Azimuth(lEle.StartPoint.X, lEle.StartPoint.Y, lEle.EndPoint.X, lEle.EndPoint.Y) + 3.1415926 / 2)
    Lpoints(0).Y = lEle.StartPoint.Y + lblWidth * Sin(Azimuth(lEle.StartPoint.X, lEle.StartPoint.Y, lEle.EndPoint.X, lEle.EndPoint.Y) + 3.1415926 / 2)
    Lpoints(1).X = lEle.EndPoint.X + lblWidth * Cos(Azimuth(lEle.StartPoint.X, lEle.StartPoint.Y, lEle.EndPoint.X, lEle.EndPoint.Y) + 3.1415926 / 2)
    Lpoints(1).Y = lEle.EndPoint.Y + lblWidth * Sin(Azimuth(lEle.StartPoint.X, lEle.StartPoint.Y, lEle.EndPoint.X, lEle.EndPoint.Y) + 3.1415926 / 2)

    Set cLine1 = CreateLineElement1(Nothing, Lpoints)
    ActiveModelReference.AddElement cLine1
    cLine1.Rewrite
    Lpoints(0).X = lEle.StartPoint.X + lblWidth * Cos(Azimuth(lEle.StartPoint.X, lEle.StartPoint.Y, lEle.EndPoint.X, lEle.EndPoint.Y) - 3.1415926 / 2)
    Lpoints(0).Y = lEle.StartPoint.Y + lblWidth * Sin(Azimuth(lEle.StartPoint.X, lEle.StartPoint.Y, lEle.EndPoint.X, lEle.EndPoint.Y) - 3.1415926 / 2)
    Lpoints(1).X = lEle.EndPoint.X + lblWidth * Cos(Azimuth(lEle.StartPoint.X, lEle.StartPoint.Y, lEle.EndPoint.X, lEle.EndPoint.Y) - 3.1415926 / 2)
    Lpoints(1).Y = lEle.EndPoint.Y + lblWidth * Sin(Azimuth(lEle.StartPoint.X, lEle.StartPoint.Y, lEle.EndPoint.X, lEle.EndPoint.Y) - 3.1415926 / 2)

    Set cLine2 = CreateLineElement1(Nothing, Lpoints)
    ActiveModelReference.AddElement cLine2
    cLine2.Rewrite
       
       
       interPoints1 = bspLine.GetIntersectionPointsOnIntersector(cLine1, Matrix3dIdentity)
       interPoints2 = bspLine.GetIntersectionPointsOnIntersector(cLine2, Matrix3dIdentity)
       interPointsM = bspLine.GetIntersectionPointsOnIntersector(lEle, Matrix3dIdentity)
       If UBound(interPoints1) = UBound(interPoints2) Then
       For lblI = LBound(interPoints1) To UBound(interPoints1)
        oeColor = bspLine.Color
        Set etxtEle = CrtElevationText(CurElevation, interPoints1(lblI), Azimuth(interPoints1(lblI).X, interPoints1(lblI).Y, interPoints2(lblI).X, interPoints2(lblI).Y), cScale, oeColor)
        clvlGrp.AddMember etxtEle
        interPoints1(lblI).Z = 0
        interPoints2(lblI).Z = 0
        interPointsM(lblI).Z = 0
        bspLine.PartialDelete NewEle1, NewEle2, interPoints1(lblI), interPoints2(lblI), interPointsM(lblI), msdView1
        bspLine.Redraw msdDrawingModeTemporaryErase
        ActiveModelReference.RemoveElement bspLine
        NewEle1.Color = oeColor
        NewEle1.Level = FindLevel("等高线计曲线")
        ActiveModelReference.AddElement NewEle1
        NewEle1.Rewrite
        clvlGrp.AddMember NewEle1
        
        NewEle2.Color = oeColor
        NewEle2.Level = FindLevel("等高线计曲线")
        ActiveModelReference.AddElement NewEle2
        NewEle2.Rewrite
        clvlGrp.AddMember NewEle2
        
        clvlGrp.Rewrite
       Next
       End If
    ActiveModelReference.RemoveElement cLine1
    ActiveModelReference.RemoveElement cLine2
       End If
    Loop
    ActiveModelReference.RemoveElement lEle
End Sub


'创建计曲线高程标注文本
Function CrtElevationText(str As String, midpoint As Point3d, rAng As Double, cScale As Single, txtColor As Integer) As TextElement
Dim txtTmp As TextElement
Dim txtPoint As Point3d
Dim tSize As Double
tSize = 3 * cScale
txtPoint.X = midpoint.X + cScale * 1.5 * Cos(rAng + 3.1415926 / 2) '- Len(str) * tSize / 2 * Abs(Cos(rAng)) '+ 3 * 0.5 'curScale
txtPoint.Y = midpoint.Y + cScale * 1.5 * Sin(rAng + 3.1415926 / 2) 'Len(str) * tSize / 2 * Abs(Sin(rAng)) + 3 * cScale 'curScale
Set txtTmp = CreateTextElement1(Nothing, str, txtPoint, Matrix3dFromAxisAndRotationAngle(2, rAng))
Set txtTmp.TextStyle = adTextStyle("设计线标注", "宋体", 0.4, 0.4)
txtTmp.TextStyle.Height = tSize
txtTmp.TextStyle.Width = tSize
txtTmp.TextStyle.Color = txtColor
txtTmp.Level = FindLevel("等高线计曲线")
ActiveModelReference.AddElement txtTmp
txtTmp.Rewrite
txtTmp.Redraw
Set CrtElevationText = txtTmp
End Function

Sub CreateShortLineProfile(spoint As Point3d, epoint As Point3d, LevelName As String)
Dim Lpoints(1) As Point3d
Dim cLine As LineElement

Lpoints(0) = spoint
Lpoints(1) = epoint

Set cLine = CreateLineElement1(Nothing, Lpoints)
cLine.Level = FindLevel(LevelName)
cLine.Color = 5
cLine.LineStyle = ActiveDesignFile.LineStyles("4")
cLine.LineWeight = 1
ActiveModelReference.AddElement cLine
cLine.Rewrite
cLine.Redraw
End Sub

Function ChkEleExist(id As DLong, appName As String, LevelName As String) As Boolean
Dim ckEe As ElementEnumerator
Dim ckScanCri As ElementScanCriteria
Set ckScanCri = New ElementScanCriteria
ChkEleExist = False
ckScanCri.ExcludeAllLevels
ckScanCri.IncludeLevel FindLevel(LevelName)
Set ckEe = ActiveModelReference.GraphicalElementCache.Scan(ckScanCri)
Do While ckEe.MoveNext
If DLongComp(ckEe.Current.id, id) = 0 Then
ChkEleExist = True
Exit Do
End If
Loop
End Function

Sub SectionInit()
If SectionLoaded = False Then
CommandState.StartDefaultCommand
CadInputQueue.SendCommand "MDL SILENTLOAD SECTION;", False
SendKeys ("%{F4}")
SectionLoaded = True
End If
End Sub
```