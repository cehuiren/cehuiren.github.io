---
title: VBA Output Message Boxes
author: QinDong
date: 2007-03-26 10:36:00 +0800
categories: 编程开发 VBA
tags: 示例代码 对话框 输出 en
---
* content
{:toc}

Message Boxes are a means to quickly display information to the user.  Message boxes can also used to get basic Yes/No or Boolean type user input. Using Message boxes to get this kind of input can eliminate the need to develop custom UserForms to gather that information.

The **MsgBox** function returns a **VbMsgBoxResult** value which is an enumerated list with each item  corresponding the the button on the message box dialog selected by the user. The following code snippets illustrate the working with the MsgBox function to collect user input.

```vb
Dim msgResult As VbMsgBoxResult

' Display Yes and No buttons
    msgResult = MsgBox("Do you want to continue?", vbYesNo, "My Macro")
    If msgResult = VbMsgBoxResult.vbYes Then
            'User clicked Yes button
    Else
            'User clicked No button
    End If
```

![](/img/2022/2022-10-29-15-15-27.png)

```vb
' Display OK and Cancel buttons
    msgResult = MsgBox("Do you want to continue?", vbOKCancel, "My Macro")
    If msgResult = VbMsgBoxResult.vbOK Then
            'User clicked OK button
    Else
            'User clicked Cancel button
    End If
```

![](/img/2022/2022-10-29-15-15-49.png)


```vb
' Display Abort, Retry, and Ignore buttons
    msgResult = MsgBox("Do you want to continue?", vbAbortRetryIgnore, "My Macro")
    Select Case msgResult
        Case VbMsgBoxResult.vbAbort
            'User clicked Abort button
        Case VbMsgBoxResult.vbIgnore
            'User clicked Ignore button
        Case VbMsgBoxResult.vbRetry
            'User clicked Retry button
    End Select
```

![](/img/2022/2022-10-29-15-16-05.png)