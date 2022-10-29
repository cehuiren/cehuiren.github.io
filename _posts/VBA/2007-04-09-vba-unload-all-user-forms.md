---
title: Unload All UserForms
author: QinDong
date: 2007-04-09 14:33:00 +0800
categories: 编程开发 VBA
tags: 示例代码 卸载 en
excerpt: 
---
* content
{:toc}

**UserForms** in VBA are the primary way to gather user input required for your macro. More complex macros may require the loading and unloading of multiple forms for gathering different types of input, particularly if you are trying to create a “wizard” type of interface. Managing the showing, hiding, and unloading of forms for multi-form macros can be a challenge.

This is particularly true when trying to exit a macro. A VBA macro will continue executing if there is a UserForm loaded. The simple Sub displayed below will unload every UserForm your macro has loaded.

```vb
Sub UnloadAllForms()
    Dim myForm As UserForm
    For Each myForm In UserForms
        Unload myForm
    Next
End Sub
```

This code snippet would be a very useful addition to an ExitMacro routine as discussed in other VBA tips.