---
layout: post
title:  "MicroStation VBA-Automatic Subroutine Loading"
date: 2020-02-05
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA 加载 代码 
excerpt: MicroStation VBA自动加载代码。
author: 中国优先社区
---
* content
{:toc}

he naming of a subroutine generally does not matter. One exception, however, is the name OnProjectLoad. If a subroutine gets this name, it is automatically started as soon as it is loaded.
This makes it much easier to use when you start MicroStation to load a VBA project because not only does it load, it runs.
Here  is a small example:

```vb
Sub  OnProjectLoad ()
    MsgBox "Hello, I was just loaded"
    ActiveWorkspace.AddConfigurationVariable "new_variable", "testvalue", True
End Sub
```

If a dialog box is opened, it means that the subroutine has executed. A configuration variable is then set and written in the UCF file.Since this subroutine is executed automatically at startup, the project needs to be loaded automatically. It must be listed in the variables **MS_VBAAUTOLOADPROJECTS**.
You can also edit the variable or simply make a hook in the project editor as shown circled here:

<div style="text-align:center;"><img src="/img/2022/2022-09-17-14-03-51.png"></div>

When you restart MicroStation, the OnProjectLoad routine is automatically loaded and executed, as seen below:

<div style="text-align:center;"><img src="/img/2022/2022-09-17-14-04-04.png"></div>

Not only does this screenshot show that the routine was executed, but that the subroutine is already running during the startup of MicroStation. The splash-screen of MicroStation is still behind the message box so the subroutine has already been carried out before the MicroStation Manager to select a drawing appears.
It is important at this point to note that although commands can be executed, commands relating to the contents of a drawing lead to errors. It is still possible,however the installation of an event handler that catches the opening of a drawing is required. More information about this can be found in the following article.