---
title: MicroStation VBA示例-CadInputQueue GetInput
author: QinDong
date: 2022-11-12 10:53:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 示例代码
excerpt: 
---
* content
{:toc}

### CadInputQueue Example
This example illustrates a technique for setting and getting the active color, and for sequencing events in MicroStation's input queue. It uses the property ActiveSettings. ColorName to get and set the name of MicroStation's active color. It uses the GetInput method to get input from the user. It uses SendCommand and SendDataPoint to send input to MicroStation. 

```vb
Sub PlaceRedLines()
    Dim saveActiveColor

    saveActiveColor = ActiveSettings.ColorName
    ActiveSettings.ColorName = "Red"

    On Error GoTo DoneSub

    With CadInputQueue
        Dim message As CadInputMessage

        .SendCommand "place line"

        Do While True
            Set message = .GetInput(msdCadInputTypeDataPoint, msdCadInputTypeReset)
            If message.InputType = msdCadInputTypeDataPoint Then
                .SendDataPoint message.point
            ElseIf message.InputType = msdCadInputTypeReset Then
                Exit Do
            End If
        Loop
    End With

DoneSub:
    '  Set MicroStation back to a nice state for the user
    ActiveSettings.ColorName = saveActiveColor
    CommandState.StartDefaultCommand

End Sub
```
{: .nolineno}

### GetInput Method Example
This example illustrates a technique for waiting for input. It uses the GetInput method of CadInputQueue to wait for the input. The input is returned in a CadInputMessage object. This examples uses the InputType property of CadInputMessage to determine what type of input occurred. Then it uses the Keyin, CommandKeyin, and Point properties to examine the input. 

```vb
Sub GetInput()
    Dim oMessage As CadInputMessage

    Application.ShowCommand "Running GetInput example"
    Application.ShowPrompt "Enter any input. Enter reset to exit"
    Application.ShowStatus ""

    Do While True
        '
        '  Wait for input.  To limit the types of input allowed, specify
        '  the desired types.  For example,
        '
  '       GetInput (msdCadInputTypeDataPoint, msdCadInputTypeKeyin)
        '
        Set oMessage = CadInputQueue.GetInput

        '
        '   Now process the message
        '
        Select Case oMessage.InputType

        Case msdCadInputTypeKeyin
            ShowStatus "Got the keyin: " & oMessage.Keyin

        Case msdCadInputTypeCommand
            ShowStatus "Got the command: " & oMessage.CommandKeyin

        Case msdCadInputTypeReset
            ShowStatus "got reset, will exit now"
            CommandState.StartDefaultCommand
            Exit Sub

        Case msdCadInputTypeDataPoint
            Dim point As Point3d
            point = oMessage.point

            With point
                ShowStatus "x = " & .X & ", y = " & .Y & ", z = " & .Z
            End With

        End Select
    Loop

End Sub
```
{: .nolineno}