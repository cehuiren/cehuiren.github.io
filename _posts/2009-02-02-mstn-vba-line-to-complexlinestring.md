---
title: MicroStation VBA创建线段生成线串示例代码
author: 网格转载
date: 2009-02-02 09:31:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 代码 线段 线串
excerpt: 
---
* content
{:toc}

以下代码先创建相连的线段，然后生成一个复杂线串对象。

```vb
Dim profileElements(3) As ChainableElement

Set profileElements(0) = CreateLineElement2(Nothing, Point3dFromXYZ(0.0591, 0.0245, 0), Point3dFromXYZ(0.0836, 0.0806, 0))
Set profileElements(1) = CreateLineElement2(Nothing, Point3dFromXYZ(0.0836, 0.0806, 0), Point3dFromXYZ(0.0899, 0.0792, 0))
Set profileElements(2) = CreateLineElement2(Nothing, Point3dFromXYZ(0.0899, 0.0792, 0), Point3dFromXYZ(0.0899, 0.0203, 0))
Set profileElements(3) = CreateLineElement2(Nothing, Point3dFromXYZ(0.0899, 0.0203, 0), Point3dFromXYZ(0.0591, 0.0245, 0))

Dim oProfile As ComplexStringElement
Set oProfile = CreateComplexStringElement1(profileElements())
```