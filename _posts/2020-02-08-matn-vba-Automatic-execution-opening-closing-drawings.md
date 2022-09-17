---
layout: post
title:  "Automatic execution when opening or closing drawings"
date: 2020-02-08
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA 代码 自动运行 
excerpt: MicroStation VBA打开、关闭设计图纸时自动运行的程序代码。
author: 中国优先社区
---
* content
{:toc}

We are often faced with the task of having to carry out actions during the opening or closing of drawings. These actions can include making adjustments or launching control routines that check and correct drawings.
Here is a small example created largely from the MicroStation VBA Help that is modified and compiled.
In the previous article explaining automatically loading VBA routines, it has already been noted that it is possible to automatically execute something, but this happens before the drawing file is opened.
Therefore, this example can bee seen more as an extension. Because there is an event handler installed in this example, after running the VBA routine, every time you open or close a drawing, an event is intercepted. This makes it easy to implement something to run later when these events occur.
These events are indicated by a message box in this example, but this can be changed accordingly for your own use.
A module with the subroutine OnProjectLoad might look something like this:

```vba
Dim oOpenClose As clsOpenClose
Under  OnProjectLoad ()
        Set oOpenClose = New clsOpenClose
        MsgBox "Eventhandler geladen"
End Sub
```

To load this VBA routine when you start MicroStation, it must be listed under the MS_VBAAUTOLOADPROJECTS, and the clsOpenClose module must also be loaded.
Here is a class module clsOpenClose that must be created separately in the defined class modules:

```vba
Dim WithEvents hooks As Application

Private Sub Class_Initialize()
    Set hooks = Application
End Sub
Private  Sub  hooks_OnDesignFileClosed ( ByVal  DesignFileName As  String )
    MsgBox ActiveDesignFile.FullName + "closed"
End Sub
 
Private  Sub  hooks_OnDesignFileOpened ( ByVal  Design Filename As  String )
    MsgBox ActiveDesignFile.FullName + "opened"
End Sub
```

In this example, two events OnDesignFileOpened and OnDesignFileClosed are intercepted when opening and closing a drawing. They both open a message box indicating whether the file has been opened or closed.
For simplicity, this example is stored as a complete VBA project and can be downloaded here:
[VBA routine to install and event handler](https://communities.bentley.com/communities/user_communities/bentley_general_de/m/bentleygeneralde-files/188840/download.aspx)