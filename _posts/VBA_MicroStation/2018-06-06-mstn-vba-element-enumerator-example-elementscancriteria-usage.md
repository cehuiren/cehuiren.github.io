---
title: MicroStation VBA Element Enumerator
author: QinDong
date: 2022-10-02 08:58:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 元素 对象 枚举 选择集
excerpt: 
---
* content
{:toc}

Many MicroStation vba’s use the **ElementScanCriteria** to build a list of elements in a model that match a set of selection criteria. The **ElementEnumerator** object is then used to iterate through all of the elements in the list. The sub below is a common example of this.

```vb
Sub EE_Example()
Dim ee As ElementEnumerator
Dim es As ElementScanCriteria
'
' set element scan criteria to find only text elements
'
Set es = New ElementScanCriteria
es.ExcludeAllTypes
es.IncludeType msdElementTypeText
'
' set enumerator from active model
'
Set ee = ActiveModelReference.Scan(es)
'
' loop through array and replace text
'
Do While ee.MoveNext
ee.Current.AsTextElement.Text = "New Text"
ee.Current.Rewrite
Loop
End Sub
```

This can create problems if the elements are modified during this iteration as this list is dynamic. Modifying elements can affect the ordering of the list causing some elements to be processed multiple times and others to be skipped entirely. 

When you are required to modify elements it is safer to create an element array from the **ElementEnumerator** object and iterate through array. The sub below has been modified to use the array technique.

```vb
Sub EE_Example()
Dim ee As ElementEnumerator
Dim es As ElementScanCriteria
Dim elArray() As Element
Dim i As Long
Dim iStart As Long
Dim iEnd As Long
'
' set element scan criteria to find only text elements
'
Set es = New ElementScanCriteria
es.ExcludeAllTypes
es.IncludeType msdElementTypeText
'
' set enumerator from active model
'
Set ee = ActiveModelReference.Scan(es)
'
' get an element array of all elements found
'
elArray = ee.BuildArrayFromContents
iStart = LBound(elArray)
iEnd = UBound(elArray)
'
' loop through array and replace text
'
For i = iStart To iEnd
elArray(i).AsTextElement.Text = "New Text"
elArray(i).Rewrite
Next
End Sub
```