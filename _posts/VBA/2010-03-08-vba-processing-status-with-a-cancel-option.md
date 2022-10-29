---
title: VBA Processing Status With a Cancel Option
author: QinDong
date: 2010-03-08 11:33:00 +0800
categories: 编程开发 VBA
tags: 示例代码 en
excerpt: 
---
* content
{:toc}

Many applications provide a progress bar with a Cancel option for long running processes. VBA does not have a native progress bar control, but you can provide the same type of information with your own “Status” UserForm containing an informational label and a Cancel button.

![](/img/2022/2022-10-29-14-44-56.png)

The code below shows how this can be accomplished. The click event of the Cancel button will set a public Boolean variable (IsCancelled) to true, which is then picked up by the main processing loop and stops the execution.The key is to include calls to the **DoEvents** function inside your code. DoEvents temporarily suspends operation of the current module so Windows can receive and process other events. In this case we want Windows to update the label on our status form and process any click event on the Cancel button.

### Main Module
```vb
Public IsCancelledAs Boolean
Sub TestCancel()
    Dim i As Long Dim iStart As Long Dim iEnd As Long
    iStart = 0
    iEnd = 2147483647   ' largest possible Long value
    With frmStatus
        .lblStatus.Caption = "Initializing"
        .Show False' Status form is non-modal
    End With
    IsCancelled = False
    For i = iStartToiEnd
        DoEvents
        If IsCancelled Then
            MsgBox "User cancelled at "&CStr(i), vbInformation
            Exit Sub
        End If
        frmStatus.lblStatus.Caption = "Counting: " &CStr(i) & " of " &CStr(iEnd)
    Next
    Unload frmStatus
    MsgBox "Processing Complete", vbInformation
End Sub
```

### Forms - frmStatus
```vb
Private Sub cmdCancel_Click()
    IsCancelled = True
    Unload Me
End Sub
```
{: .nolineno }

Calling **DoEvents** and updating the status label every time through the loop is a big performance drain on the application. Modifying the main processing loop so that the DoEvents and status update happen at only regular intervals will greatly speed up the processing of your application. In the modified snippet below DoEvents and the status update are done every 1000 times through the loop.

```vb
iStatus = 1000
For i = iStartToiEnd
    If i Mod iStatus = 0 Then
        DoEvents
        If IsCancelled Then
            MsgBox "User cancelled at "&CStr(i), vbInformation
            Exit Sub
        End If
        frmStatus.lblStatus.Caption = "Counting: " &CStr(i) & " of " &CStr(iEnd)
        End If
Next
```
{: .nolineno }