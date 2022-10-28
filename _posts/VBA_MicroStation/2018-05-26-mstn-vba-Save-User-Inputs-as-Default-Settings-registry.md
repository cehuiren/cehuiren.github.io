---
title: Save User Inputs as Default Settings
author: la
date: 2018-05-26 15:58:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 注册表 设置
excerpt: 
---
* content
{:toc}

Many times in a macro you want to save user input so that it is stored and then recalled as the program’s default values the next time they run your application. This can be done by writing these values to an ini file, or some other formatted ascii file, then reading and decoding that file the next time the user starts your application. That is the hard way, the easy way is to use the Windows Registry. VBA has a special location set aside in the Windows Registry for storing VBA program settings:

**HKEY_CURRENT_USER\Software\VB and VBA Program Settings**

VBA provides the **GetSetting** and **SaveSetting** functions for storing and retrieving your macro’s settings from this location.

### GetSetting
The VBA **GetSetting** function returns a value stored in the Windows Registry. The registry values are stored and returned as String values. Any numeric, or other non-string values, must be converted to the proper type prior to assigning them to your variables or controls.

```vb
stringValue = GetSetting(appname, section, key, default)
```

The **GetSetting** function has the following arguments:
- **appname** – the name of your program or macro
- **section** – a way to subdivide your saved settings, it is usually best to use the form name or class name
- **key** – the control name, member name, or variable name of the item being stored
- **default** – the default value to use if the setting is not found in the registry

### SaveSetting
The VBA **SaveSetting** function stores a value in the Windows Registry. Since the registry entries are stored as String values any numeric, or other non-string values, must be converted to strings prior to saving.
SaveSetting appname, section, key, setting

The **SaveSetting** function has the following arguments:

- **appname** – the name of your program or macro
- **section** – a way to subdivide your saved settings, it is usually best to use the form name or class name
- **key** – the control name, member name, or variable name of the item being stored
- **setting** – the value being saved in the registry, saved as a String value.


### Example
The example below is the code behind a UserForm and illustrates the use of the GetSetting and SaveSetting functions.  When the UserForm is loaded (or initialized) the **UserForm_Initialize** sub is called and reads in our default values from the registry. After the user clicks on the form’s OK button the **cmdOK_Click** sub is executed and the form settings are saved to the macro’s registry location.

The UserForm has three controls:

**txtFileName**       – a Text Box for storing the name of a file

**ckReadOnly**      – a Check Box storing our Boolean value

**cmdOK**             – a Command Button used to trigger saving the values to the registry

You will also see that this example is storing and recalling the screen position of the user form (Top and Left values).  This allows you to place the UserForm in the last location where the user had it positioned. Since the Top and Left are Long data types the **Cstr()** function must be used to convert them to strings when saving to the registry.  Likewise the **CLng()** function must be used when retrieving the values out of the registry to convert them from strings back to longs.

```vb
Option Explicit
Private Sub UserForm_Initialize()
    Me.txtFileName.Text = GetSetting _
("MyMacro", "Settings", "FileName", "")
    Me.ckReadOnly.Value = CBool(GetSetting _
("MyMacro", "Settings", "ReadOnly", "True"))
    Me.Top = CLng(GetSetting _
("MyMacro", "Settings", "FormTop", "0"))
    Me.Left = CLng(GetSetting _
("MyMacro", "Settings", "FormLeft", "0"))
End Sub
Private Sub cmdOK_Click()
    SaveSetting "MyMacro", "Settings", _
"FileName", Me.txtFileName.Text
    SaveSetting "MyMacro", "Settings", _
"ReadOnly", CStr(Me.ckReadOnly.Value)
    SaveSetting "MyMacro", "Settings", _
"FormTop", CStr(Me.Top)
    SaveSetting "MyMacro", "Settings", _
"FormLeft", CStr(Me.Left)
    Unload Me
End Sub
```

The image below shows the saved registry settings in the Registry Editor window.

![](/img/2022/2022-10-28-14-15-37.png)