---
title: MicroStation VBA Unit Round Off
author: QinDong
date: 2018-05-02 11:07:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 单位 精度 四舍五入
excerpt: 
---
* content
{:toc}

In MicroStation the round off of working units is controlled by the **Settings > Design File > Working Units > Accuracy** setting. This applies to both coordinate input and display in MicroStation tools. Unfortunately there is no automatic conversion for us in VBA. The example function below demonstrates a method that can be used to access the design file working unit accuracy setting for displaying coordinates.

```vb
Public Function CoordinateReadout(dValue As Double) As String
    Select Case ActiveSettings.CoordinateAccuracy
    Case MsdCoordinateAccuracy.msdAccuracy0
        CoordinateReadout = Format$(dValue, "#0.")
    Case MsdCoordinateAccuracy.msdAccuracy1
        CoordinateReadout = Format$(dValue, "#0.0")
    Case MsdCoordinateAccuracy.msdAccuracy2
        CoordinateReadout = Format$(dValue, "#0.00")
    Case MsdCoordinateAccuracy.msdAccuracy3
        CoordinateReadout = Format$(dValue, "#0.000")
    Case MsdCoordinateAccuracy.msdAccuracy4
        CoordinateReadout = Format$(dValue, "#0.0000")
    Case MsdCoordinateAccuracy.msdAccuracy5
        CoordinateReadout = Format$(dValue, "#0.00000")
    Case MsdCoordinateAccuracy.msdAccuracy6
        CoordinateReadout = Format$(dValue, "#0.000000")
    Case Else
        CoordinateReadout = CStr(dValue)
    End Select
End Function
```
>Note: The function above does not handle of display values using fractions or scientific notation.
{: .prompt-info}