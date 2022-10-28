---
title: Create a Solid of Rotation using MicroStation VBA
author: 网格转载
date: 2009-02-02 09:36:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 剖面 实体 代码
excerpt: 
---
* content
{:toc}

This page lists some solutions to common MicroStation VBA (MVBA) problems. Tips are published as examples and are not necessarily working code.

## SmartSolids
Bentley Systems add the **SmartSolid** class to MicroStation VBA in a late version of MicroStation V8i. It continues to be available with MicroStation CONNECT. There are two related classes. The comments are taken from MicroStation VBA help …

- **SmartSolid**: Holds generic methods useful in dealing with SmartSolid objects
- **SmartSolidElement**: A kind of Element that corresponds to a MicroStation SmartSolid/SmartSurface element

In other words, the **SmartSolid** class is an implementation of mathematical concepts that help us create and manipulate 3D solid objects. The **SmartSolidElement** class provide tools to handle a 3D solid as a MicroStation DGN element.

## Solid of Rotation
We can create a solid of rotation by first identifying or creating a 2D profile. Next, we rotate the profile about a given point, in a given orientation, by a specified angle. The VBA method is **SmartSolid.RevolveProfile**.

### 2D Profile
Look at the VBA documentation for **SmartSolid.RevolveProfile**: Revolves a wire or sheet body about the axis passing through the point and along the axis.

The profile element is a DGN element: profile body which is swept along the path, it should not be a closed curve.

A profile is a two-dimensional open shape. You can create such an element from a simple line-string or other primitive curve, or a complex string. Here's an example …

```vb
Dim profileElements(3) As ChainableElement

Set profileElements(0) = CreateLineElement2(Nothing, Point3dFromXYZ(0.0591, 0.0245, 0), Point3dFromXYZ(0.0836, 0.0806, 0))
Set profileElements(1) = CreateLineElement2(Nothing, Point3dFromXYZ(0.0836, 0.0806, 0), Point3dFromXYZ(0.0899, 0.0792, 0))
Set profileElements(2) = CreateLineElement2(Nothing, Point3dFromXYZ(0.0899, 0.0792, 0), Point3dFromXYZ(0.0899, 0.0203, 0))
Set profileElements(3) = CreateLineElement2(Nothing, Point3dFromXYZ(0.0899, 0.0203, 0), Point3dFromXYZ(0.0591, 0.0245, 0))

Dim oProfile As ComplexStringElement
Set oProfile = CreateComplexStringElement1(profileElements())
```

### Rotation Parameters
Next, decide how to rotate that profile. We need a point to rotate about, an axis of rotation, and a rotation angle (the angle, as is usual, is measured in Radians) …

```vb
Dim pointOfRotation As Point3d
pointOfRotation = Point3dZero

Dim xAxis As Vector3d
'  Rotate about the X-axis
xAxis = Vector3dFromXYZ(1, 0, 0)

Dim rotateAngle As Double
'  Rotate by Pi radians (180°)
rotateAngle = Pi
```

### Rotate Profile
Now everything is ready. We're going to create a **SmartSolidElement** that can be added to a DGN model. We create that **SmartSolidElement** using the **RevolveProfile** method of class **SmartSolid**. The actual rotation is quite simple …

```vb
Dim oBodySolid As SmartSolidElement
Set oBodySolid = SmartSolid.RevolveProfile(oProfile, pointOfRotation, xAxis, rotateAngle)
```

### Cap Surface
Finally, you may want to call **SmartSolidElement.CapSurface**. That ensures that you create a 3D solid rather than a 3D surface. Or, as VBA help puts it: Converts a sheet body to a solid by adding faces to fill up "hollow spaces".

```vb
oBodySolid.CapSurface
ActiveModelReference.AddElement oBodySolid
```

Here's a screenshot of the resulting solid …

![](/img/2022/2022-09-20-09-52-22.png)
_Rotated Profile_

## Problems
Some people have reported problems using **SmartSolidElement.RevolveProfile**. Here are some comments …

- The profile of rotation must be an open element. It should not be a closed shape
- The angle of rotation is specified in radians, not degrees. Note that if you intend to rotate by, say, 180° and specify 180 for the rotation, that is about 10313°, which is quite a few rotations and probably not what you intended. VBA provides a conversion function **Radians()**, so you've no excuse for not using radians
- The axis of rotation is define by a vector. If you want to rotate about the Z axis that vector is 0,0,1 (the example above rotates about the X axis)
- The origin and range of the resulting solid must conform to MicroStation's rules for solids, in particular the solid working area

MicroStation VBA developer Paul Rowlands commented, after struggling with **SmartSolidElement.RevolveProfile**: Cause of the problem is global origins set at large values which a certain infrastructure client likes to use in their seed files

## Questions
Post questions about MicroStation programming to the [MicroStation Programming Forum](https://communities.bentley.com/products/programming/microstation_programming/f/microstation-programming---forum).