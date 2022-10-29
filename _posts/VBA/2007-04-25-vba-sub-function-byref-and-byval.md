---
title: Byref and Byval
author: QinDong
date: 2007-04-25 10:30:00 +0800
categories: 编程开发 VBA
tags: 示例代码 函数 过程 en
excerpt: 
---
* content
{:toc}

The **ByRef** and **ByVal** keywords are used when passing variables to subroutines and functions. Passing arguments by reference (**ByRef**) passes the memory location of the variable which means that any changes made to the variable during the procedure are permanent. Passing arguments by value (**ByVal**) only passes a copy of the variable so that any changes to the variable during the procedure are only made on the copy. If neither qualifier is specified the variable is passed **ByRef**.

### Example
In the example below, three variables **(i,j,k)** are passed to the sub AddOne. The variable i is passed with no qualifier, j is passed **ByRef**, and k is passed **ByVal**. The values for all three variables are printed out after returning from the sub. Note that the value for i is updated just as the value for j while the value for k is unchanged.

```vb
Public Sub MyMacro()
    Dim i As Long
    Dim j As Long
    Dim k As Long   
    i = 10
    j = 10
    k = 10
    AddOne i, j, k
    Debug.Print "i=" & CStr(i) & ", j=" & CStr(j) & ", k=" & CStr(k)
End Sub

Private Sub AddOne(i As Long, ByRef j As Long, ByVal k As Long)
    i = i + 1
    j = j + 1
    k = k + 1
End Sub
```

![](/img/2022/2022-10-29-15-10-34.png)