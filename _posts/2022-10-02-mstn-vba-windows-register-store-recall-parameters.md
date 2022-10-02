---
title: MicroStation VBA注册表保存、取出参数
author: QinDong
date: 2022-10-02 08:58:00 +0800
categories: 编程开发 VBA-MicroStation
tags: 参数 设置 注册表 代码
excerpt: 
---
* content
{:toc}

![](/img/2022/2022-10-02-08-42-53.png)

## 注册关键字总结

作用 | 关键字
--- | ---
删除程序设置 | DeleteSetting
读入程序设置 | GetSetting, GetAllSettings
保存程序设置 | SaveSetting

### SaveSetting 语句
        
在 Windows 注册表中 或 (Macintosh中)应用程序初始化文件中的信息保存或建立应用程序项目。

#### 语法
`SaveSetting appname, section, key, setting`

**SaveSetting** 语句的语法具有下列命名参数：

部分 | 描述
--- | ---
appname | 必要。字符串表达式，包含应用程序或工程的名称，对这些应用程序或工程使用设置 在Macintosh中，这是System文件夹中Preferences文件夹中初始化文件的文件名。
section | 必要。字符串表达式，包含区域名称，在该区域保存注册表项设置。
key | 必要。字符串表达式，包含将要保存的注册表项设置的名称。
setting | 必要。表达式，包含 key 的设置值。

#### 说明
如果无论如何也不能保存注册表项设置，则将导致错误发生。

### SaveSetting 语句示例
下列示例首先使用 **SaveSetting** 语句来建立 Windows 注册区（或 16 位 Windows 平台的.ini 档）里 MyApp 应用程序的项目，然后使用 **DeleteSetting** 语句来将之删除。

```vb
' 在注册区中添加一些设置值。
SaveSetting appname := "MyApp", section := "Startup", _
            key := "Top", setting := 75 
SaveSetting "MyApp","Startup", "Left", 50 
' 删除区段及所有的设置值。
DeleteSetting "MyApp", "Startup" 
```

### GetSetting 函数
        
从 Windows 注册表中 或 (Macintosh中)应用程序初始化文件中的信息的应用程序项目返回注册表项设置值。

### 语法

`GetSetting(appname, section, key[, default]) `

**GetSetting** 函数的语法具有下列命名参数：
部分 | 描述
--- | ---
appname | 必要。字符串表达式，包含应用程序或工程的名称，要求这些应用程序或工程有注册表项设置。 在Macintosh中，这是System文件夹中Preferences文件夹中初始化文件的文件名。	
section | 必要。字符串表达式，包含区域名称，要求该区域有注册表项设置。	
key	必要。字符串表达式，返回注册表项设置的名称。	
default | 可选。表达式，如果注册表项设置中没有设置值，则返回缺省值。如果省略，则 default 取值为长度为零的字符串 ("")。	

#### 说明
_如果 GetSetting 的参数中的任何一项都不存在，则 GetSetting 返回 default 的值。_

### GetSetting 函数示例
本示例首先使用 **SaveSetting** 语句来建立Windows注册区（或 16位 Windows 平台的.ini档）里 **appname** 应用程序的项目，然后使用 **GetSetting** 函数来得到其中一项设置并显示出来。因为有传入参数 **default**，**GetSetting** 函数一定会有返回值。请注意，**section** 名称不能用 **GetSetting** 函数取得。最后，使用 **DeleteSetting** 语句将该应用程序项删除。

```vb
' 用来保存 GetSetting 函数所返回之二维数组数据的变量。
Dim MySettings As Variant
' 在注册区中添加项目。
SaveSetting "MyApp","Startup", "Top", 75
SaveSetting "MyApp","Startup", "Left", 50
Debug.Print GetSetting(appname := "MyApp", section := "Startup", _
                       key := "Left", default := "25")
DeleteSetting "MyApp", "Startup"
```

### GetAllSettings 函数
        
从 Windows 注册表 或 (Macintosh中)应用程序初始化文件中的信息中返回应用程序项目的所有注册表项设置及其相应值（开始是由 **SaveSetting** 产生）。

#### 语法
`GetAllSettings(appname, section) `

**GetAllSettings** 函数的语法具有下列命名参数：
部分 | 描述
--- | ---
appname | 必要。字符串表达式，包含应用程序或工程的名称，并要求这些应用程序或工程有注册表项设置 在Macintosh中，这是System文件夹中Preferences文件夹中初始化文件的文件名。	
section | 必要。字符串表达式，包含区域名称，并要求该区域有注册表项设置。GetAllSettings 返回 Variant，其内容为字符串的二维数组，该二维数组包含指定区域中的所有注册表项设置及其对应值。	

#### 说明
_如果 **appname** 或 **section** 不存在，则 **GetAllSettings** 返回未初始化的 **Variant**。_

### GetAllSettings 函数示例
本示例首先使用 **SaveSetting** 语句来建立 Windows注册区里 **appname** 应用程序的项目，然后再使用 **GetAllSettings** 函数来取得设置值并显示出来。请注意，应用程序名和 **section** 名称不能用 **GetAllSettings** 函数取得。最后，使用 **DeleteSetting** 语句将该应用程序项删除。

```vb
' 用来保存 GetAllSettings 函数所返回之二维数组数据的变量
' 整型数是用来计数用。
Dim MySettings As Variant, intSettings As Integer
' 在注册区中添加设置值。
SaveSetting appname := "MyApp", section := "Startup", _
key := "Top", setting := 75
SaveSetting "MyApp","Startup", "Left", 50
' 取得输入项的设置值。
MySettings = GetAllSettings(appname := "MyApp", section := "Startup")
    For intSettings = LBound(MySettings, 1) To UBound(MySettings, 1)
        Debug.Print MySettings(intSettings, 0), MySettings(intSettings, 1)
    Next intSettings
DeleteSetting "MyApp", "Startup"
```

### DeleteSetting 语句
        
在 Windows 注册表中 或 (Macintosh中)应用程序初始化文件中的信息，从应用程序项目里删除区域或注册表项设置。

#### 语法 
`DeleteSetting appname, section[, key]`

**DeleteSetting** 语句的语法具有下列命名参数：
部分 | 描述
--- | ---
appname | 必需的。字符串表达式，包含应用程序或工程的名称，区域或注册表项用于这些应用程序或工程。 在Macintosh中，这是System文件夹中Preferences文件夹中初始化文件的文件名。	
section | 必要。字符串表达式，包含要删除注册表项设置的区域名称。如果只有 appname 和 section，则将指定的区域连同所有有关的注册表项设置都删除。	
key | 可选。字符串表达式，包含要删除的注册表项设置。	

#### 说明
_如果提供了所有参数，则删除指定的注册表项设置。如果试图使用不存在的区域或注册表项设置上的 DeleteSetting 语句，则发生一个运行时错误。_

### DeleteSetting 语句示例

下列示例先使用 **SaveSetting** 语句，来建立 Windows 注册区（或 16 位 Windows 平台的 **ini** 文件）里 MyApp 应用程序的项目，然后使用 **DeleteSetting** 语句将之删除。因为没有指定 key 参数，整个区段都会被删除掉，包括区段名称及其所有的机码（key）。
```vb
' 在注册区中添加一些设置值。
SaveSetting appname := "MyApp", section := "Startup", _
            key := "Top", setting := 75 
SaveSetting "MyApp","Startup", "Left", 50 
' 删除区段及所有的设置值。
DeleteSetting "MyApp", "Startup" 
```