---
layout: post
title:  "MicroStation VBA-创建直线代码(CreateLineTest)"
date: 2020-02-03
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA 直线 代码 
excerpt: MicroStation VBA创建直线代码。
author: 测量老覃
---
* content
{:toc}

This function requires 2 points to create a line, which we then may place into the drawing. A VBA code example using this function may look like this:

```vba
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