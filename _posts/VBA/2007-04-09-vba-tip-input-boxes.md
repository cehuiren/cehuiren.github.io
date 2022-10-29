---
title: VBA TIP：Input Boxes
author: QinDong
date: 2007-04-09 15:36:00 +0800
categories: 编程开发 VBA
tags: 示例代码 输入 en
excerpt: 
---
* content
{:toc}

Like a Message Box, the **InputBox** displays a modal dialog with a message/prompt string and a tile. With an **InputBox** there are nooptions for what buttons to display. The **InputBox** always displays a text field to capture user text input. If the user selects the **OK** button in the **InputBox** dialog the string in the text field is returned as a string.

The basic **InputBox** function syntax is as follows:

**InputBox(Prompt string, Dialog title, Default string value)**

### Example:

```vb
Dim strInput As String
strInput = InputBox("Enter distance", "MyMacro", "5")
```

![](/img/2022/2022-10-29-15-05-50.png)