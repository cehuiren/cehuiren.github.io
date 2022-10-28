---
title: Simple example with object library
author: 中国优先社区
date: 2018-05-26 13:58:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 示例代码 对象库
excerpt: 
---
* content
{:toc}

### Simple example – Using MicroStation object library

If a VBA code is created from a recording, the base of the code are keyin commands like this one from the recorded example: **CadInputQueue.SendKeyin " PLACE SMARTLINE"**

Often VBA macros are used for repeated use, e.g. to import thousands of coordinates and place lines or cells for each  together with tags or Itemtypes. It is recommended then to use functions from the MicroStation object library instead for a better control and performance.

The alternate method to create elements are functions starting with “create”. To search for such “Create*” functions we can use the help file or directly browse in the MicroStation object library. From the VBA Editor with F2 we can switch from Source Code display to an Explorer like display of a complete list of all classes, functions, methods and properties. F7 brings you back to source code display.

The first view of the object library may look like this:

![](/img/2022/2022-10-28-14-21-18.png)

By default the content of <All libraries> is displayed. You may select here to get a filtered content of a specific library:

![](/img/2022/2022-10-28-14-21-26.png)

“MicroStationDGN” – list all classes added with MicroStation “VBA” – list all base functionality from VBA
The list may provide more libraries from other application like Excel, if these libraries are referenced to this project (See Tools > References) A search for “create” in the library returns a long list with functions. During scrolling down there should be 2 functions to create lines: CreateLineElement1 and CreateLineElement2.

A click on the selection provides details  about the declaration of the function:

![](/img/2022/2022-10-28-14-21-34.png)

This function requires 2 points to create a line, which we then may place into the drawing. A VBA code example using this function may look like this:

```vb
Sub CreateLineTest()
Dim oLine As LineElement
Dim pStart As Point3d
Dim pEnd As Point3d
    pStart = Point3dFromXYZ(10, 20, 30)
    pEnd = Point3dFromXYZ(70, 40, 20)
    Set oLine = CreateLineElement2(Nothing, pStart, pEnd)
    ActiveModelReference.AddElement oLine
End Sub
```

A brief description of this subroutine: The function CreateLineElement2 takes 2 points and returns an element of Type LineElement. The method .AddElement is finally used to add this new created element into the active model of the active drawing file.

参见 [原文]（https://communities.bentley.com/products/programming/microstation_programming/w/wiki/48720/simple-example-with-object-library）