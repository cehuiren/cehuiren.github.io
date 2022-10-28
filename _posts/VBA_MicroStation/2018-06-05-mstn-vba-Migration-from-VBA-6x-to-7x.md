---
title: VBA Migration from VBA 6.x to 7.x
author: QinDong
date: 2018-06-05 10:41:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 兼容 升级
excerpt: 
---
* content
{:toc}

>TIP: Some Information below can be located in the MicroStationVBA.chm help file.  It is recommended to become familiar with these important Help Topics:
{: .prompt-info}

- "Changes for 64-bit Processes"
- "Calling DLL functions from VBA"
- "Object Model Change History"
- "MicroStation VBA FAQ"

The VBA integration with MicroStation was introduced with the V8 product generation. VBA 6.x is supported in the V8 generation until V8i

With the new CONNECT Edition VBA 7 is required to allow 64-Bit application support and declare functions from the 64 Bit host application. With the migration of VBA projects from 32 to 64 Bit support there are some changes with VBA 7.1.

### Changes for VBA 7.1

VBA 7.1 introduces support for working in a 64-bit program. There are a few changes in VBA 7.1 to help programs work with native code in a 64-bit process.

In a 64-bit process a pointer is 64 bits; in a 32-bit process it is 32 bits. VBA 7.1 introduces the type LongLong to represent a 64-bit pointer. It introduces the type LongPtr that is treated as a Long in a 32-bit process and a LongLong in a 64-bit process. LongPtr is the preferred type for declaring a variable to hold a native pointer.

An existing declaration that declares all pointers as Long will not work reliably in a 64-bit process. It will compile correctly but is very likely to cause a crash. It may crash the first time it is used, or it may just crash intermittently. To help programmers cope with this, Microsoft introduced the keyword PtrSafe. When running in a 64-bit process the VBA compiler generates an error any time it sees a Declare statement that does not include PtrSafe. This forces the programmer to examine every use of Declare to decide what arguments are pointer arguments, replacing Long with LongPtr as necessary.

VBA 6 does not support LongPtr or PtrSafe so code that is shared between VBA 6 and VBA 7 must be conditionally defined. Microsoft has introduced a built-in conditional compilation argument Vba7 to assist with this. For example:

```vb
#If Vba7 Then 
Declare PtrSafe Sub CopyMemoryToVBA Lib "kernel32" _ 
     Alias "RtlMoveMemory" _ 
     (ByRef VBALocation As Any, _ 
     ByVal SourceLoc As LongPtr, _ 
     ByVal length As Long) 
#Else 
Declare Sub CopyMemoryToVBA Lib "kernel32" _ 
     Alias "RtlMoveMemory" _ 
     (ByRef VBALocation As Any, _ 
     ByVal SourceLoc As Long, _ 
     ByVal length As Long) 
#EndIf
```

**Declare Statement:**

`[Public | Private] Declare PtrSafe Sub name Lib "libname" [Alias "aliasname"] [([arglist])]`

`[Public | Private] Declare PtrSafe Function name Lib "libname" [Alias "aliasname"] [([arglist])] [As type]`

**Some COM Components Not Available in 64-bit Process**

If a COM component server is a DLL or an OCX the pointer size of the COM DLL/OCX and the process must match. VBA hosted in a 64-bit process (like MicroStation CONNECT Edition) does not let the VBA program reference a 32-bit DLL/OCX. Microsoft Windows Common Controls is an example of a 32-bit OCX that cannot be used in a 64-bit process. 

Microsoft has not provided a 64-bit version of this OCX, so the COM library is not available to a VBA program running in a 64-bit process.

Some COM components run in-process and others run out-of-process. Typically, if a DLL or OCX file provides a COM component, then the component runs in-process. If an EXE provides a COM component, then the component runs out-of-process.

If a COM component runs out-of-process then pointer sizes do not have to match. Therefore, a 32-bit process can use the MicroStationDGN object model from a MicroStation process regardless of whether the MicroStation process is a 32-bit MicroStation or a 64-bit MicroStation. Likewise, a 64-bit process can use the MicroStationDGN object model regardless of whether the MicroStation process is a 32-bit MicroStation or a 64-bit MicroStation
All DLL's loaded into a process must use the same size addresses. Therefore, a DLL using 32-bit addresses cannot be used in a 64-bit process.

For more information see:

- [64-Bit Visual Basic for Applications Overview](http://msdn.microsoft.com/en-us/library/office/gg264421(v=office.15).aspx)
- [Compatibility Between the 32-bit and 64-bit Versions of Office](http://msdn.microsoft.com/en-us/library/office/ee691831(v=office.14).aspx#odc_office2010_Compatibility32bit64bit_ActiveXControlCOMAddinCompatibility%20/)