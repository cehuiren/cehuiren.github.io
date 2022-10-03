---
title: MicroStation操作技巧集锦
author: QinDong
date: 2022-10-03 16:36:00 +0800
categories: MicroStation 基础
tags: key-in 快捷键 ACS
excerpt: 
---
* content
{:toc}

### 根据对象属性创建显示规则

![](/img/2022/2022-10-03-16-39-24.png)

### Alt+鼠标data键，将对象属性设为当前激活属性；

![](/img/2022/2022-10-03-16-39-33.png)

### 快捷键：

**Alt+Data键**：匹配对象属性；

**Shift+试探**：捕捉工作窗口；

![](/img/2022/2022-10-03-16-39-44.png)

**Ctrl+试探**：设计原点

**Alt+试探**：三维数据

**Shift+Reset**：视图工具窗口

![](/img/2022/2022-10-03-16-39-50.png)

**Xbutton(中键)**：平移；

**Shift+Xbutton**：旋转视图（按住Shift）；

**Ctrl+Xbutton**：摇转视图（按一下Ctrl即可）；

**Alt+Xbutton**：绕光标旋转视图（按一下Alt即可）；

### 自定义工具栏备份

打开Key-in，输入以下命令：

备份：`ribbon customizations exportecxml`

导入：`ribbon customizations importecxml`

### 将文字压印到曲面对象

![](/img/2022/2022-10-03-16-40-02.png)

可视化、主页、实用工作、压印（印章图标），将视图调整到压印方向，按视图方向压印。

![](/img/2022/2022-10-03-16-40-08.png)

### 将ACS重置为全局原点

![](/img/2022/2022-10-03-16-40-14.png)

### 变量设置：影射高程值属性

`MS_ELEVPROP_AS_Z=AGL`；其中**AGL**为含有高程值的属性项。

### 字段型(field)

关联对象发生变化后字段内容不变的更新方法：

**Key-in**：`field update all file`

### 脚本命令中等待坐标输入、最后数据点

#### Wait for a Data Point
When building keyin scripts, add `%d` to instruct MicroStation to wait for your datapoint.

For example:
```
Place Fence Block;%d;%d;Fence Move
```

Starts the Place Fence Block tool, waits for you to issue two data points, and then starts the Fence Move tool. 

#### Remember the last data point

Here's a fun little tip!Stick `XY=#,#,#` on either a function key or custom button.Activating it will get you back to the last datapoint. 

### MicroStation激活深度与ACS平面捕捉

利用激活深度与ACS平面捕捉锁可以捕捉指定高程的关键点，如在顶视图输入

`AZ=10`，然后打开ACS平面捕捉锁，则以后绘图捕捉的点其高程为10。

### 匹配属性

`Alt+Data` : Match Element

### 放置表格

MSTN中放置表格：

![](/img/2022/2022-10-03-16-40-24.png)