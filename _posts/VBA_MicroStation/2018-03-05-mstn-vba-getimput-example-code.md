---
title: MicroStation VBA GetInput
author: QinDong
date: 2018-03-05 08:58:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 输入
excerpt: 
---
* content
{:toc}

For those of you still wanting to convert MicroStation User Commands (UCM’s) to VBA
one way to ease your transition is to use the VBA CadInputQueue.GetInput method for
getting input from the user. This method most closely mimics the GET function in the
UCM language.

The example function below shows how you can use the GetInput method for placing a
fence block. There is also a good example of using GetInput in the MicroStation VBA
Help examples.

```vb
Public Function PlaceFenceBlock() As Boolean
Dim ciMessage As CadInputMessage
Dim IsFirstPoint As Boolean
PlaceFenceBlock = False
IsFirstPoint = True
CadInputQueue.SendKeyin "Place Fence Block"
Do While True
Set ciMessage = CadInputQueue.GetInput
Select Case ciMessage.InputType
'
' User Keyin Input
'
Case msdCadInputTypeKeyin
CadInputQueue.SendKeyin ciMessage.Keyin
'
' Users selects a different MicroStation command
'
Case msdCadInputTypeCommand
CadInputQueue.SendReset
CommandState.StartDefaultCommand
Exit Function
'
' User clicks Reset
'
Case msdCadInputTypeReset
CadInputQueue.SendReset
'
' If the user has not clicked any data points
' then exit the function
' otherwise reset flag to the first point
'
If IsFirstPoint Then
CommandState.StartDefaultCommand
Exit Function
Else
IsFirstPoint = True
End If
'
' User clicks Data Point
'
Case msdCadInputTypeDataPoint
CadInputQueue.SendDataPoint ciMessage.Point
End Select
If IsFirstPoint Then
IsFirstPoint = False
Else
Exit Do
End If
Loop
PlaceFenceBlock = True
End Function
```

>Note: Use caution if using the GetInput method in combination with UserForms as
incorrect implementation can cause MicroStation lock up.
{: .prompt-info}