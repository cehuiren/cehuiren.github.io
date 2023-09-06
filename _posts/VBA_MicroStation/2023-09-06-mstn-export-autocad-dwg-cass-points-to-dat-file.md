---
title: MicroStation VBA读取AutoCAD CASS地形图坐标点写入数据文件
author: QinDong
date: 2023-09-06 17:18:00 +0800
categories: 编程开发 VBA-MicroStation
tags: CASS 地形图 坐标
excerpt: 
---
* content
{:toc}

MicroStation直接打开AutoCAD中绘制的南方CASS地形图，使用此VBA程序代码将图中地形点导出到.dat数据文件中。

```vb
Sub sel_cass_points()
Dim el As Level
Dim sc As ElementScanCriteria
Dim ee As ElementEnumerator
Dim e_num As Long

Dim point_coor As String
Dim px As Double
Dim py As Double
Dim ph As Double

Dim datFileName As String
Dim fileNum As Long
datFileName = fi1eOpenCreate("*.dat", "F:\", "保存数据文件")

If Trim(datFileName) <> "" Then
    '点编号
        e_num = 1
        fileNum = FreeFile()
    
    Open datFileName For Output As fileNum
        
        Set el = ActiveModelReference.Levels("GCD")
        Set sc = New ElementScanCriteria
        
        sc.ExcludeAllLevels
        sc.ExcludeAllTypes
        
        sc.IncludeType msdElementTypeSharedCell
        sc.IncludeLevel el
        
        Set ee = ActiveModelReference.Scan(sc)
        
    Do While ee.MoveNext
            Dim ecass As SharedCellElement
            Set ecass = ee.Current.AsSharedCellElement
            'ee.Current.Redraw msdDrawingModeHilite
        If ecass.Level.Name = "GCD" Then
            '点坐标
            px = ecass.Origin.X
            py = ecass.Origin.Y
            ph = ecass.Origin.Z
            point_coor = e_num & ",," & Round(px, 3) & "," & Round(py, 3) & "," & Round(ph, 3)
            Print #fileNum, point_coor
            e_num = e_num + 1
        End If
    Loop
    
    Close #fileNum

End If
End Sub
```