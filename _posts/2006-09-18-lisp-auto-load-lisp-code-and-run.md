---
title: 关于自动加载 AutoLISP 应用程序
author: 网格转载
date: 2006-09-18 13:49:10 +0800
categories: 编程开发 AutoLisp
tags: AutoLisp 帮助
excerpt: 
---
* content
{:toc}

要自动加载 AutoLISP 例程，请将它们添加到 AutoCAD **启动组**，或者使用 **acad.lsp** 文件加载它们。

### 添加到启动组
1. 运行 **APPLOAD（命令）**。
2. 在启动组下，单击内容按钮。
3. 单击添加按钮。
4. 浏览到 LISP 文件的位置，选择它，然后单击打开按钮。
5. 将所有 LISP 例程添加到启动组后，单击关闭按钮。
6. 再次单击关闭以关闭“加载/卸载应用程序”对话框。

### 使用 CUI 加载
1. 运行 **CUI（命令）**
2. 选择“acad.cuix”（或自定义局部 .cuix）。
3. 选择 LISP 文件，然后单击鼠标右键。
4. 从关联菜单中选择加载 LISP。
5. 浏览到要添加的 LISP 的位置，然后选择该文件。
6. 单击“应用并关闭”以退出 CUI 编辑器。

![](/img/2022/2022-09-21-14-40-37.png)
_使用 CUI 加载 LISP 例程_

### 使用 acad.lsp 文件
AutoCAD 启动时总是会加载 acad.lsp 文件。 如果 acad.lsp 中定义了特殊函数 S::STARTUP，将会执行该函数。
例如，每次运行 AutoCAD 时，都要加载两个名为 stair.lsp 和 wall.lsp 的 AutoLISP 例程。创建一个包含以下代码行的 acad.lsp 文件，并将其放在 AutoCAD 支持路径中。

```lisp
(defun s::startup ()
(load "STAIR.LSP")
(load "WALL.LSP")
)
```

如果 _wall.lsp_ 和 _stair.lsp_ 位于 AutoCAD 搜索路径中，它们将会自动加载。如果 AutoLISP 例程不在 AutoCAD 支持路径中，请在 acad.lsp 文件中包含完整路径。使用“/”或“\\”作为路径分隔符。在相同示例中，acad.lsp 文件应如下所示：

```lisp
(defun s::startup ()
(load "C:/PROG/LISP/STAIR.LSP")
(load "C:\\PROG\\LISP\\WALL.LSP")
)
```

如果这样定义 **S::STARTUP** 函数，当有其他应用程序也使用 **S::STARTUP** 函数时（例如某个第三方插件），可能会出现问题。为了确保兼容性，请在存在现有的 **S::STARTUP** 函数时附加代码。要执行此操作，请添加以下代码：

```lisp
(defun mystartup ()
(load "C:/PROG/LISP/STAIR.LSP")
(load "C:\\PROG\\LISP\\WALL.LSP")
)
(if s::startup
(setq s::startup (append s::startup (quote ((mystartup)))))
(defun s::startup () (mystartup))
)
```

注意：每次启动 AutoCAD 时，都会运行 **acad.lsp** 文件。每次打开图形时，都会运行 **acaddoc.lsp** 文件。