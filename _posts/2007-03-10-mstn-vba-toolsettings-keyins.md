---
layout: post
title:  "TOOLSETTINGS KEY-INS"
date: 2007-03-10
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA key-in 代码 
excerpt: MicroStation VBA编程执行键入命令。
author: 网络转载
---
* content
{:toc}

The **CadInputQueue.SendCommand** and **CadInputQueue.SendKeyin** methods are easy ways for programmers to automate MicroStation tasks in their VBA macros.  One problem that users encounter is how to adjust the **Tool settings** for a particular command. As it turns out there is a key-in command that can help with that as well.
To find the Tool settings key-ins for an individual tool select the tool from the menu. In this example we are using Place Block. With the tool active open the **Key-in** window and enter the string: set item toolsettings but do not hit Enter. The available toolsettings key-ins for the active tool will be displayed in the far right list box. Those key-ins can be used in your macro with the SendKeyin method to ensure the toolsettings are configured correctly.

<div style="text-align:center;"><img src="/img/2022/2022-09-17-14-55-07.png"></div>

```vb
    With CadInputQueue
        'Start place block tool
        .SendCommand "place block icon"
        'set the block type to Orthogonal
        .SendKeyin "set item toolsettings placeblocktype=0"
        'set the area to Solid
        .SendKeyin "set item toolsettings elementarea=0"
        'set the fill type to Opaque
        .SendKeyin "set item toolsettings elementfill=1"
        'set the fill color to 3
        .SendKeyin "set item toolsettings elementfillcolor=3"
    End With
```