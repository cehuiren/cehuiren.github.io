---
title: Use Constants For Common Values
author: QinDong
date: 2010-05-03 09:26:00 +0800
categories: 编程开发 VBA
tags: 示例代码 常数 en
---
* content
{:toc}

Some developers will calculate common values in code every time they are needed in an application. This slows down your application as it has to calculate the value every time.

Others will enter in the straight numeric value, which is just frowned upon programming practice. The best way to handle these values is to use VBA Constants.

```vb
'Common degree values in Radians
Public Const RAD_90 As Double = 1.5707963267949
Public Const RAD_180 As Double = 3.1415926535898
Public Const RAD_270 As Double = 4.7123889803847
Public Const RAD_360 As Double = 6.2831853071796
'Geometric related constants
Public Const PI As Double = 3.14159265358979
Public Const PId2 As Double = 1.5707963267949
Public Const PIx2 As Double = 6.28318530717959
Public Const ONE_THIRD As Double = 0.33333333
Public Const FOUR_THIRDS As Double = 1.33333333
```

When the constants are defined in a code module they can be used anywhere in your project like in the following routines.

```vb
Public Function GetBearingAngle(ByVal deltaX As Double, ByVal deltaY As Double, _
    ByRef strNS As String, ByRef strEW As String) as Double
    Dim dAngle As Double
    dAngle = Atn2(deltaX, deltaY)
    Select Case True
    Case (dAngle <= RAD_90) 
        strNS = "N" 
        strEW = "E" 
    Case (dAngle <= RAD_180) 
        strNS = "S" 
        strEW = "E" 
        dAngle = RAD_180 - dAngle 
    Case (dAngle <= RAD_270) 
        strNS = "S" 
        strEW = "W" 
        dAngle = RAD_90 - ((dAngle - RAD_270) * -1) 
    Case Else 
        strNS = "N" 
        strEW = "W" 
        dAngle = RAD_360 - dAngle 
    End Select 
    'Angle is returned in decimal degrees, must be converted to DMS 
    GetBearingAngle = dAngle 
End Sub 

Public Function AreaCircle(ByVal dRadius As Double) As Double 
    AreaCircle = PI * (dRadius ^ 2) 
End Function 

Public Function CircumferenceCircle(ByVal dRadius As Double) As Double 
    CircumferenceCircle = 2 * PI * dRadius 
End Function 

Public Function AreaSphere(ByVal dRadius As Double) As Double 
    AreaSphere = 4 * PI * (dRadius ^ 2) 
End Function 

Public Function VolumeSphere(ByVal dRadius As Double) As Double 
    VolumeSphere = FOUR_THIRDS * PI * (dRadius ^ 3) 
End Function
```
{: .nolineno }