---
title: MicroStation VBA-OpenDesignFileForProgram
author: 中国优先社区
date: 2009-03-13 11:36:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 代码 VBA MicroStation 文件
excerpt: 
---
* content
{:toc}

OpenDesignFileForProgram and MicroStation's Dependency State

```vb
1. Private Declare PtrSafe Function mdlDependency_processAffected Lib "stdmdlbltin.dll" () As Long  
2.   
3. '  
4. '  For design files loaded via OpenDesignFileForProgram it is sometimes necessary  
5. '  to call UpdateElementDependencyState  
6. '  
7. Private Sub UpdateDependencyPre0800903()  
8. ' UpdateElementDependencyState was added in 8.9.3. Code that must be  
9. ' backwards compatible can use the MDL function  
10. mdlDependency_processAffected  
11. End Sub  
12.   
13. Private Sub UpdateDependency0800903()  
14. UpdateElementDependencyState  
15. End Sub  
16.   
17. Public Sub ScanOpenDgnForProgram()  
18. Dim oFile As DesignFile  
19. Dim sFullFileName As String  
20.   
21. sFullFileName = ActiveDesignFile.Path & "\Test.dgn"  
22. Set oFile = OpenDesignFileForProgram(sFullFileName)  
23. Call DoScan(oFile)  
24. oFile.Close  
25. End Sub  
26.   
27. Private Sub DoScan(ByRef oFile As DesignFile)  
28. Dim oElement As Element  
29. Dim oClosed As ClosedElement  
30. Dim oSc As New ElementScanCriteria  
31.   
32. oSc.ExcludeAllTypes  
33. oSc.IncludeType msdElementTypeShape  
34.   
35. Dim iCountClosedShapes As Integer  
36. Dim iCountShapesWithTags As Integer  
37.   
38. Dim oScanEnumerator As ElementEnumerator  
39. Set oScanEnumerator = oFile.DefaultModelReference.Scan(oSc)  
40.   
41. UpdateDependency0800903  
42.   
43. Do While oScanEnumerator.MoveNext  
44. Set oElement = oScanEnumerator.Current  
45. If oElement.IsClosedElement Then  
46. iCountClosedShapes = iCountClosedShapes + 1  
47. Set oClosed = oElement  
48. If oElement.HasAnyTags Then  
49. iCountShapesWithTags = iCountShapesWithTags + 1  
50. End If  
51. Debug.Print iCountClosedShapes & " - " & iCountShapesWithTags  
52. End If  
53. Loop  
54.   
55. Debug.Print "Number of Closed Shapes: " & iCountClosedShapes  
56. Debug.Print "Number of Closed Shapes with Tags: " & iCountShapesWithTags  
57. End Sub
```