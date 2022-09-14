---
layout: post
title:  "OpenRoads Designer工作环境行业标准、公司和项目标准设置"
date: 2022-08-30
categories: OpenRoads
tags: OpenRoads 设置 标准
excerpt: OpenRoads Designer（简称ORD）的标准遵守继承规则，制定和设置好相关标准层级及工作空间，能更好的管理图纸文件和有关样式标准。
author: QinDong
---
* content
{:toc}

![](/img/2022/2022-09-03-11-07-46.png)

所在目录：**C:\ProgramData\Bentley\OpenRoads Designer CE\Configuration**

行业级、公司级（WorkSpace）通过复制目录与配制文件完成，项目级（WorkSet）在ORD初始界面中对应WorkSpace中直接创建。

### 1、行业层级标准（目录Organization-Civil内）

**C:\ProgramData\Bentley\OpenRoads Designer CE\Configuration\Organization-Civil**

进入此目录后将目录“_Civil Default Standards - Metric”和文件“_Civil Default Standards - Metric.cfg” **复制并原地粘贴更名为“水利水电行业标准”和“水利水电行业标准.cfg”：

![](/img/2022/2022-09-03-11-09-39.png)

如上即完成行业层级标准的创建。

### 2、公司层级标准（目录WorkSpace内）

进入**C:\ProgramData\Bentley\OpenRoads Designer CE\Configuration\WorkSpaces**

将目录“Metric Standards”和文件“Metric Standards.cfg”复制并原地粘贴更名为“Water12”、“Water12.cfg”如下图：

![](/img/2022/2022-09-03-11-09-48.png)

用记事本打开“Water12.cfg”将“CIVIL_ORGANIZATION_NAME”变量值设定为以上行业标准名称“水利水电行业标准”：

![](/img/2022/2022-09-03-11-10-00.png)

如上完成公司层级标准的创建并与行业层级标准建立关联。

### 3、项目层级标准（WorkSet）

启动软件，在“Water12”工作空间内创建工作集，按项目创建，设计文件夹、标准文件夹均保持默认即可。

![](/img/2022/2022-09-03-11-10-18.png)

在项目中创建文件时不要把dgn文件放在项目的DGN目录内，一是该目录较深，二是重装后会被清除。

### 4、库文件、种子文件位置

特征库与种子文件均位于行业标准目录下：

特征库：**C:\ProgramData\Bentley\OpenRoads Designer CE\Configuration\Organization-Civil\水利水电行业标准\Dgnlib\Feature Definitions**

种子文件：**C:\ProgramData\Bentley\OpenRoads Designer CE\Configuration\Organization-Civil\水利水电行业标准\Seed**

地形特征等均包含在库文件中。

公司层级特征库文件与种子文件位置工作空间WorkSpaces下的公司名中的标准目录内：

库：**C:\ProgramData\Bentley\OpenRoads Designer CE\Configuration\WorkSpaces\Water12\Standards\Dgnlib\Feature Definitions**

种子：**C:\ProgramData\Bentley\OpenRoads Designer CE\Configuration\WorkSpaces\Water12\Standards\Seed**

### 5、自定义行业标准、公司标准及相关文件备份与恢复说明

备份：C:\ProgramData\Bentley\OpenRoads Designer CE\Configuration，将此目录压缩备份（即在目录C:\ProgramData\Bentley\OpenRoads Designer CE中）成Configuration.zip

恢复：将Configuration.zip解压后复制到C:\ProgramData\Bentley\OpenRoads Designer CE目录中。