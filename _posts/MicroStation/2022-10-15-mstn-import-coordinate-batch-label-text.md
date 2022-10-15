---
title: MicroStation利用Import Coordinate工具批量导入文本
author: 中国优先社区
date: 2022-10-15 13:56:00 +0800
categories: MicroStation 基础
tags: 文本 文字 导入
excerpt: 
---
* content
{:toc}

### 需求说明
如下截图所示，用户拥有一个勘探得到的Excel表格，其中记录了文本标识的内容和对应的xyz坐标值。用户希望批量的将表格中的文本标识写入到dgn中对应的坐标点上。可以使用MicroStation 自带的Import Coordinate工具，进行批量写入。

![](/img/2022/2022-10-15-13-55-10.png)

### 操作方法
1、Import Coordinate工具需要使用txt格式文件，所以先拷贝Excel的数据部分（第一行标题栏不拷贝），新建一个Text文本文件，将拷贝内容粘贴到里面并进行保存。

![](/img/2022/2022-10-15-13-55-19.png)

2、保存Text文本时，coding方式请选择默认的ANSI方式。

![](/img/2022/2022-10-15-13-55-27.png)

3、然后按照如下截图调用Import Coordinate工具，导入txt文件，注意格式是根据xyz的顺序，比如此例中是yxz的顺序，就选择TYXZ方式。

![](/img/2022/2022-10-15-13-55-36.png)