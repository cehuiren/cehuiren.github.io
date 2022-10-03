---
title: MicroStation CE版中使用Legacy可见边显示方式
author: QinDong
date: 2022-10-02 17:58:00 +0800
categories: MicroStation 基础
tags: 横断面
excerpt: 
---
* content
{:toc}

在 MicroStation V8i 中，当我们在 **Drawing** 或 **Sheet** 模型中参考了 **Design** 模型时（组图的过程），参考文件对话框中 **Visible Edges**（可见边）这一栏会有 **Dynamic**、**Cached** 和 **Legacy** 三种选项，如下图所示：

![](/img/2022/2022-10-02-17-09-39.png)

在 **CE** 版本中，这个 **Visible Edges** 选项默认只有 **Dynamic** 和 **Cahced** 两种，如何能在 CONNECT 版本中出现 **Legacy** 选项呢？可通过设置配置变量`MS_REF_ENABLE_LEGACY_VISEDGES`为 1 来实现，如下图所示：

![](/img/2022/2022-10-02-17-09-47.png)