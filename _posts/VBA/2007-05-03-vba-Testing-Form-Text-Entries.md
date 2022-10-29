---
title: Testing Form Text Entries
author: QinDong
date: 2010-05-03 09:26:00 +0800
categories: 编程开发 VBA
tags: 示例代码 文本框 en
---
* content
{:toc}

When using VBA forms for gathering text inputs you may want to test the values. This is particularly true when your application is expecting numeric text inputs. The examples below show two TextBox events that can be used to validate numeric entries. The default Change event tests the input after every key press. This even can create problems when you want to allow for negative values, floating point numbers less than one, or simply clearing out an existing value. The single characters of “-“ and “.” along with a zero length text string are not recognized as numeric values. In this case it may be better to use the AfterUpdate TextBox event which tests the values only when focus is moved to another form control.

```vb
'
' Tests value after every character entry
'
Private Sub TextBox1_Change()
    If Not IsNumeric(TextBox1.Text) Then
        MsgBox "Text entry must be a number", vbExclamation, "TextBox1"
    End If
End Sub
'
' Tests value after leaving the text box
'
Private Sub TextBox1_AfterUpdate()
    If Not IsNumeric(TextBox1.Text) Then
        MsgBox "Text entry must be a number", vbExclamation, "TextBox1"
    End If
End Sub
```