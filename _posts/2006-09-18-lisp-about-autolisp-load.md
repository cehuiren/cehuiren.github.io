---
title: 关于加载 AutoLISP 应用程序
author: 网格转载
date: 2006-09-18 13:49:10 +0800
categories: 编程开发 AutoLisp
tags: AutoLisp 帮助
excerpt: 
---
* content
{:toc}

## 关于加载 AutoLISP 应用程序
--- 
需要将 AutoLISP 文件加载到 AutoCAD，然后才能使用它们。

AutoLISP 应用程序存储在可编辑的 ASCII 文本文件中，其扩展名为 .lsp。这些文件通常有一个标题部分，用于说明程序及其用法，以及其他特别说明。该标题可能还包括注释，用于记录关于使用该程序的作者和版权信息。注释以分号 (;) 开始。可以用文本编辑器或能生成 ASCII 文本文件的字处理器来查看和编辑这些文件。

**重要信息** 从基于 AutoCAD 2014 的产品开始，当 SECURELOAD 系统变量设定为 1 或 2 时，自定义应用程序必须在安全模式下工作。在安全模式下进行操作时，程序限制为从“支持文件搜索路径”中的受信任位置加载和执行包含代码的文件；受信任的位置由 TRUSTEDPATHS 系统变量指定。有关详细信息，请参见“关于防止恶意代码”。
{: .prompt-warning}

AutoLISP 应用程序必须先加载后才能使用。可以用 **APPLOAD** 命令或 **AutoLISP load** 函数来加载应用程序。加载 AutoLISP 应用程序会将 AutoLISP 代码从 LSP 文件加载到系统内存中。如果 LSP 文件不位于“支持文件搜索路径”中，则必须在 Filename 参数中指定一个相对支持路径。

![](/img/2022/2022-09-21-14-28-50.png)
_Appload加载应用程序对话框_

用 **load** 函数加载应用程序需要在命令提示下输入 AutoLISP 代码。如果 **load** 函数执行成功，则在命令提示下显示文件中最后一个表达式的值。该值通常是文件中定义的最后一个函数的名称，或关于新加载的函数的用法说明。如果 load 函数执行失败，则返回一条 AutoLISP 错误消息。 **load** 失败的原因可能是文件的编码错误或是提供了错误的文件名。 

**load** 函数的语法为：

```lisp
(load filename [onfailure])
```

此语法表示 **load** 函数具有两个参数： **filename** （必需）和 onfailure （可选）。在命令提示下加载 AutoLISP 文件时，通常只需提供 filename 参数。下例将加载 AutoLISP 文件 newfile.lsp。

命令: `(load "newfile")`

.lsp 扩展名不是必需的。此格式对位于当前库路径中的任何 LSP 文件都有效。

要加载不在库路径中的 AutoLISP 文件，必须提供完整的路径和文件名作为 filename 参数。

命令: `(load "d:/files/morelisp/newfile")`

注意指定目录路径时，必须用一个斜杠 (/) 或两个反斜杠 (\\) 作为分隔符，因为单个反斜杠在 AutoLISP 中具有特殊意义。