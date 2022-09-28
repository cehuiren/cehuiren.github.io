---
title: MicroStation通过Key-in命令给元素增加Links信息
author: QinDong
date: 2022-09-28 20:38:00 +0800
categories: MicroStation 基础
tags: key-in 元素
excerpt: 
---
* content
{:toc}

在Microstation下我们可以手动操作给元素增加Links信息，Links可以使元素与磁盘上的文件关联、与URL关联、与Key-in命令关联、与dgn文件中的某一个模型关联、与dgn文件中的Saved View关联，元素和外部对象的关联方便了信息的访问，在工程设计中有一定的实用价值。

如何通过程序自动给元素增加Links信息呢？很遗憾Mdl及VBA没有公开类似的API接口。能否通过Key-in命令来实现呢，继而程序员通过Mdl的发送命令接口(如mdlInput_sendKeyin())来实现程序自动创建的目的呢？

答案是肯定的，但是这组Key-in命令并没有公开，下面介绍几组未公开的Key-in命令来实现给元素添加不同种类的Links信息：
```
添加文件Links:            ELEMENT CREATE LINK FILE D:\mydoctmp\books.xml
添加URL Links:           ELEMENT CREATE LINK URL http://www.bentley.com
添加Key-in Links:        ELEMENT CREATE LINK KEYIN "ustnkeyin:place line;xy=500,125;xy=537,165"
添加模型Links:            ELEMENT CREATE LINK DGNMODEL "C:\test.dgn" my3d
添加保存的视图Links: ELEMENT CREATE LINK DGNVIEW "C:\test.dgn" "my3d" "mySavedV1"
```

如果直接输入：`element create link`则会弹出添加链接对话框：

![](/img/2022/2022-09-28-20-36-32.png)

点击“…”选择文件，提示选择链接对象：

![](/img/2022/2022-09-28-20-36-44.png)

按“OK”后选择需要添加链接的元素对象即可，然后点击元素上的链接标志可以直接链接目标。