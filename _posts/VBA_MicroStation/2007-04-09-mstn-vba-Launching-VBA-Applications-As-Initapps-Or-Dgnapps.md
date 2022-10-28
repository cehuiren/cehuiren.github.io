---
layout: post
title:  "Launching VBA Applications As Initapps Or Dgnapps"
date: 2007-04-09
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA 加载 自动运行 
excerpt: MicroStation VBA代码自动加载运行的方法。
author: 网络转载
---
* content
{:toc}

MicroStation V8 allows users to run **MDL** applications at the startup of MicroStation using the **MS_INITAPPS** environment variable, and at the opening of a new design file with the **MS_DGNAPPS** variable. The delivered Runmacro MDL application allows you similar functionality for MicroStation BASIC macros. Now with MicroStation VBA, you can also emulate these options for users running MicroStation V8 version **08.00.02.20** or higher.

### Running VBA Application as an Initapp
There are two major steps involved in setting up your application for the startup of MicroStation V8. The first is to create a subroutine called **OnProjectLoad()**. This is the routine that gets called when MicroStation loads a MicroStation VBA project:

```vb
Option Explicit
Sub OnProjectLoad()
' Start here
MsgBox "MicroStation VBA Starting here"
End Sub
```

The next step is to check the **Auto-Load** option on in the VBA Project Manager.

Now every time you start MicroStation, your MicroStation VBA Project will load automatically.

### Running VBA Application as a Dgnapp
Setting a MicroStation VBA application to run as a Dgnapp will require you to implement the **IPrimitive** and **ILocate** events. This involves calling an instance of your class to monitor for the opening and closing of a design file. The first step is to insert a class module into your project and call it **OpenClose**:

Next, you will need to go to the top of your Module code and declare a global class variable as follows:

```vb
Option Explicit
Dim oOpenClose As OpenClose
```

The **"Option Explicit"** statement implies that the user must define each variable's datatype or you will receive an error when trying to run your code.

Next, you want to create a new instance of the class object you just defined in your subroutine:

```vb
Sub Main()
Set oOpenClose = New OpenClose
End Sub
```

Now that we have created a new instance of the class, we need to link the class into the module to look for specific events. Go back to your class and type in the following statements:

```vb
Option Explicit
Dim WithEvents hooks As Application
Private Sub Class_Initialize()
Set hooks = Application

End Sub
```

The code you entered initializes the class to monitor events in MicroStation (in our case we want to look for opening and closing of the design files). Now we have to add the code for the events and for those operations we want to perform when the event occurs. The following code is used to monitor for the open event:

```vb
Private Sub hooks_OnDesignFileOpened(ByVal dgnFileName As String)

MsgBox "Opening DGN file " + dgnFileName

End Sub
```

This event will get called by your VBA application every time a new design file is opened. The class will provide the "dgnFileName" variable for you to use. Now we need to write the code for the close event:

```vb
Private Sub hooks_OnDesignFileClosed(ByVal dgnFileName As String)

MsgBox "Closing DGN file " + dgnFileName

End Sub
```

Now that we have our logic written, the final step is to ensure that the Auto-Load option for this project is checked for the Autoload.mvba example, or else we would need to manually load and run the project ourselves.

In review, we now know how to create and automate VBA applications for Initapps and Dgnapps. They are a convenient and time-saving method to running VBA applications in MicroStation V8.

[See Also](https://communities.bentley.com/products/microstation/w/microstation__wiki/2842/2842)