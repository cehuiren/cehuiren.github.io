---
title: 关于 AutoLISP 应用程序
author: 网格转载
date: 2006-09-18 13:39:10 +0800
categories: 编程开发 AutoLisp
tags: AutoLisp 帮助
excerpt: 
---
* content
{:toc}

关于 AutoLISP 应用程序
--- 
AutoLISP 基于简单易学而又功能强大的 LISP 编程语言。由于 AutoCAD 具有内置 LISP 解释器，因此用户可以在命令提示下输入 AutoLISP 代码，或从外部文件加载 AutoLISP 代码。

AutoLISP 是用于自动执行设计任务的应用程序接口。当加载 AutoLISP 应用程序后，它在自己的名称空间中为每个打开的图形执行任务。名称空间是一个隔离的环境，用于避免特定于某一文档的 AutoLISP 应用程序与另一个图形中的程序在符号或变量名和值方面发生冲突。例如，当在每个打开的图形中执行代码时，如下代码行将为符号 a 设置不同的值。

```lisp
(setq a (getvar "DWGNAME"))
```
{: nolineno}

AutoLISP 应用程序能够提示用户输入、直接访问内置 AutoCAD 命令，以及直接在图形数据库中修改或创建对象。通过创建 AutoLISP 程序，可以向 AutoCAD 添加专用命令或由工作流驱动的命令。实际上，某些标准 AutoCAD 命令是 AutoLISP 应用程序。

用户可以选择进行试验：在命令提示下输入代码后可立即看到结果。这使 AutoLISP 语言容易试验，而不管用户的编程经验如何。

AutoLISP 提供了三种文件格式的应用程序：

- 读取 LSP 文件 (.lsp) — 包含 AutoLISP 程序代码的 ASCII 文本文件。
- 读取 FAS 文件 (.fas) — 单个 LSP 程序文件的二进制编译版本。
- 读取 VLX 文件 (.vlx) — 一个或多个 LSP 文件和/或对话框控制语言 (DCL) 文件的编译集。

**注意**：名称相似的 AutoLISP 应用程序文件的加载由它们的编辑时间决定。除非指定完整的文件名（包括文件扩展名），否则将加载最近编辑过的 LSP、FAS 或 VLX 文件。
即使用户对编写 AutoLISP 应用程序不感兴趣，程序中也包含许多有用的程序。也可以从 Internet 或第三方开发商处下载 AutoLISP 应用程序。了解如何加载和使用这些程序有助于提高生产率。
{: .prompt-info}