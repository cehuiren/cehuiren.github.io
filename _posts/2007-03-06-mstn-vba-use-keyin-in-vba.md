---
layout: post
title:  "USE KEY-IN COMMANDS WHEN LEARNING VBA"
date: 2007-03-06
categories: 编程开发 VBA-MicroStation
tags: MicroStation VBA key-in 代码 
excerpt: MicroStation VBA编程执行键入命令。
author: 网络转载
---
* content
{:toc}

Non-programmers attempting to learn VBA can be easily overwhelmed by the complicated object definitions. While familiarizing yourself with VBA utilize MicroStation key-in commands as much as possible to ease the transition. Many MicroStation key-ins can be used in VBA using the CadInputQueue.SendKeyin method. As you become more proficient in VBA you can start replacing the key-in commands with more efficient MicroStation VBA methods.
Consider the two macros below, ChangeToByLevel1 and ChangeToByLevel2. They both change all elements in the active model to the Default level and use ByLevel symbology. Most MicroStation users can understand the first macro as it uses all MicroStation key-in commands to accomplish the task. The second macro uses more complex MicroStation VBA methods to directly modify the elements.

```vb
Sub ChangeToByLevel1()
    ' 
    ' set desired symbology
     ' 
    CadInputQueue.SendKeyin "lv=default" 
    CadInputQueue.SendKeyin "co=bylevel" 
    CadInputQueue.SendKeyin "lc=bylevel" 
    CadInputQueue.SendKeyin "wt=bylevel"
    '  
   ' choose all the elements and change their symbology 
    ' 
    CadInputQueue.SendKeyin "choose all" 
    CadInputQueue.SendKeyin "change level" 
    CadInputQueue.SendKeyin "change color" 
    CadInputQueue.SendKeyin "change linestyle" 
    CadInputQueue.SendKeyin "change weight" 
    CadInputQueue.SendKeyin "choose none"
 End Sub
```

```vb
Sub ChangeToByLevel2()
     Dim es As ElementScanCriteria
     Dim ee As ElementEnumerator
     Dim lvDefault As Level 
    Dim lsByLevel As LineStyle 
    Dim elemArray() As Element 
    Dim i As Long 
    Dim iStart As Long
     Dim iEnd As Long
    ' 
    ' set the output level and linestyle objects
     ' 
    Set lvDefault = ActiveDesignFile.Levels.Find("Default") 
    Set lsByLevel = ByLevelLineStyle
    ' 
    ' define the scan criteria
     ' 
    Set es = New ElementScanCriteria
     es.ExcludeNonGraphical
    ' 
    ' create the element array
     ' 
    Set ee = ActiveModelReference.Scan(es)
     elemArray = ee.BuildArrayFromContents
    ' 
    ' change the symbology of each element 
    ' 
    iStart = LBound(elemArray) 
    iEnd = UBound(elemArray)
     For i = iStart To iEnd
         Set elemArray(i).Level = lvDefault
         elemArray(i).Color = -1
         elemArray(i).LineWeight = -1 
        Set elemArray(i).LineStyle = lsByLevel 
        elemArray(i).Rewrite 
    Next 
End Sub
```