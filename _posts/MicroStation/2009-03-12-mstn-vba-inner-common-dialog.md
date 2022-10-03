---
title: MicroStation VBA调用内置文件对话框
author: 中国优先社区
date: 2009-03-12 10:37:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 方件 对话框 MicroStation VBA 通用代码 代码
excerpt: 
---
* content
{:toc}

稍作修改：
```vb
1. Declare PtrSafe Function mdlDialog_fileOpen Lib "stdmdlbltin.dll" (ByVal fileName As String, _  
2.     ByVal fFileH As Long, ByVal resourceId As Long, ByVal suggestedFileName _  
3.     As String, ByVal filterstring As String, ByVal defaultDirectory As String, _  
4.     ByVal titlestring As String) As Long  
5.   
6. Declare PtrSafe Function mdlDialog_fileCreate Lib "stdmdlbltin.dll" (ByVal fileName As String, _  
7.     ByVal rFileH As Long, ByVal resourceId As Long, ByVal suggestedFi1eName As String, ByVal _  
8.     filterstring As String, ByVal defaultDirectory As String, ByVal titlestring As String) As Long  
9.   
10. 'Learning MicroStation VBA教程代码：C19 P440  
11. '以下对话框并不真正创建或打开文件，只是返回  
12. '一个文件名称字符串而已，真正的打开、创建操  
13. '作需代码实现  
14. '2022-04-04  
15.   
16. '内置打开对话框，只选择存在的文件  
17. Function fileOpen(filter As String, initPath As String, dlgTitle As String)  
18.     Dim strFName As String  
19.     Dim lngfhandle As Long  
20.     Dim lngrid As Long  
21.     Dim retVal As Long  
22.     strFName = Space(255)  
23.     retVal = mdlDialog_fileOpen(strFName, lngfhandle, lngrid, "", filter, initPath, dlgTitle) '*.dat;*.txt  
24.     Select Case retVal  
25.     Case 0 'open  
26.         strFName = Left(strFName, InStr(1, strFName, Chr(0)) - 1)  
27.     fileOpen = Trim(strFName)  
28.     Case 1 'cancel  
29.           fileOpen = ""  
30.     End Select  
31.       
32. End Function  
33.   
34. '内置打开创建对话框，可以输入文件名，选择存在的提示覆盖  
35. Function fi1eOpenCreate(filter As String, initPath As String, dlgTitle As String)  
36.     Dim strFName As String  
37.     Dim lngfhandle As Long  
38.     Dim lngrid As Long  
39.     Dim retVal As Long  
40.     strFName = Space(255)  
41.     retVal = mdlDialog_fileCreate(strFName, lngfhandle, lngrid, "", filter, initPath, dlgTitle)  
42.     Select Case retVal  
43.     Case 0 'Open  
44.         strFName = Left(strFName, InStr(1, strFName, Chr(0)) - 1)  
45.     fi1eOpenCreate = Trim(strFName)  
46.     Case 1 'Cancel  
47.         fi1eOpenCreate = ""  
48.     End Select  
49.  End Function
```

原始代码：
```vb
1. Declare PtrSafe Function mdlDialog_fileOpen Lib "stdmdlbltin.dll" (ByVal fileName As String, _  
2.     ByVal fFileH As Long, ByVal resourceId As Long, ByVal suggestedFileName _  
3.     As String, ByVal filterstring As String, ByVal defaultDirectory As String, _  
4.     ByVal titlestring As String) As Long  
5.   
6. Declare PtrSafe Function mdlDialog_fileCreate Lib "stdmdlbltin.dll" (ByVal fileName As String, _  
7.     ByVal rFileH As Long, ByVal resourceId As Long, ByVal suggestedFi1eName As String, ByVal _  
8.     filterstring As String, ByVal defaultDirectory As String, ByVal titlestring As String) As Long  
9.   
10. 'Learning MicroStation VBA教程代码：C19 P440  
11. '以下对话框并不真正创建或打开文件，只是返回  
12. '一个文件名称字符串而已，真正的打开、创建操  
13. '作需代码实现  
14. '2022-04-04  
15.   
16. '内置打开对话框，只选择存在的文件  
17. Sub TestFileOpen()  
18.     Dim strFName As String  
19.     Dim lngfhandle As Long  
20.     Dim lngrid As Long  
21.     Dim retVal As Long  
22.     strFName = Space(255)  
23.     retVal = mdlDialog_fileOpen(strFName, lngfhandle, lngrid, "", "*.dat;*.txt", "f:\", "Open File")  
24.     Select Case retVal  
25.     Case 0 'open  
26.         strFName = Left(strFName, InStr(1, strFName, Chr(0)) - 1)  
27.         MsgBox "File Selected:" & vbCr & strFName  
28.     Case 1 'cancel  
29.         MsgBox "No selected."  
30.     End Select  
31. End Sub  
32.   
33. '内置打开创建对话框，可以输入文件名，选择存在的提示覆盖  
34. Sub TestFi1eCreateA()  
35.     Dim strFName As String  
36.     Dim lngfhandle As Long  
37.     Dim lngrid As Long  
38.     Dim retVal As Long  
39.     strFName = Space(255)  
40.     retVal = mdlDialog_fileCreate(strFName, lngfhandle, lngrid, "", "*.dat ", " F:\", "Create Fi1eA")  
41.     Select Case retVal  
42.     Case 0 'Open  
43.         strFName = Left(strFName, InStr(1, strFName, Chr(0)) - 1)  
44.         MsgBox "Fi1eSelected: " & vbCr & strFName  
45.     Case 1 'Cancel  
46.         MsgBox "No Fi1e Selected. "  
47.         'User hit the Cancel Button  
48.     End Select  
49. End Sub  
```