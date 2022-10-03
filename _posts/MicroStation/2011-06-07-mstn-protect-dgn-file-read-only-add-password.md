---
title: MicroStation如何保护DGN文件为只读
author: QinDong
date: 2011-06-07 17:05:00 +0800
categories: MicroStation 基础
tags: 文件 加密
excerpt: 
---
* content
{:toc}

为了防止文件被其他人修改，对于V8格式的文件，可以通过给文件加保护密码使该文件对著作者以外的其他人只能以只读方式打开。

选择MicroStation的主菜单 `File > tools > Protection > Protect` 打开如下 **Protect** 窗口，选中 **With Password** 选项，然后输入密码，点击 OK 按钮确认。请注意，此处设置的密码是著作者权限密码，拥有文件的所有操作权限。

![](/img/2022/2022-10-02-17-02-20.png)

![](/img/2022/2022-10-02-17-02-37.png)

确认后下图的Digital Rights窗口会自动打开。点击上部的第二个图标“Add a password”，给文件加只读权限的密码。

![](/img/2022/2022-10-02-17-02-49.png)

如下图所示，在 **Password** 栏输入另一组密码，其他选项保持默认，即没有选择的状态即可，此处因为权限的“Edit”选项没有选中，所以用此组密码打开文件时只能以只读方式打开，没有修改，导出，打印等权限。

![](/img/2022/2022-10-02-17-02-55.png)