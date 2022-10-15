---
title: MicroStation Annotation Scale的可用性
author: 中国优先社区
date: 2022-10-15 13:48:00 +0800
categories: MicroStation 基础
tags: 注释 比例 标注 比例尺
excerpt: 
---
* content
{:toc}

Annotation Scale是一个为了在图纸里正确地表现标注的大小，比如文字标注，尺寸标注等，而用来调节它们比例尺的工具。

它是一种只对标注起作用的特殊比例尺，让您能够根据实际需要调节标注大小的强有力的工具。Annotation Scale能够用来确保任何比例图纸上的标注都清晰易读。正因为这个原因，它有时候也被指作Sheet Scale。

我们经常会关心标注在出图的比例尺。在早些时候，还没有Annotation Scale之前，在放置标注的同时就需要考虑到它的尺寸，调节尺寸时需要手动来进行。但是当标注已经被放置好以后，修改标注就是一个单调乏味并且冗长的工作，因为需要重复之前所有的步骤。而现在，Annotation Scale就让事情变得非常简单，只需要one click就可以改变所有标注显示的比例尺大小。

### 什么是Annotation
在这里，我们只将使用Annotation Scale的标注元素称作Annotation。对于一个使用Annotation Scale的尺寸标注就可以被称作一个Annotation。相反，如果没有使用Annotation Scale，那么它就不能够被称作Annotation。是否使用Annotation Scale可以在放置元素时设置，也可以在之后进行更改。如下图，在放置文字标注的时候，开启Annotation Scale开关。

![](/img/2022/2022-10-15-11-15-51.png) 

如果必要的话，Annotation Scale的开关应该被打开。V8i里可以在Setting：Locks：Annotation Scale里开启和关闭。Connect Edition里可以在Drawing：Drawing Aids：Locks里开启和关闭。

- **V8i**

![](/img/2022/2022-10-15-11-15-59.png)

- **Connect Edition**

![](/img/2022/2022-10-15-11-16-10.png) 

### 能够使用Annotation的元素
- Text
- Dimensions
- Text Nodes
- Notes
- Multi-leader Notes
- Annotation Cells
- Callouts（Sections, etc.）
- Drawing Titles
- Tags
- Line styles(Line styles需要被标记为non-physical并且在Model的属性里需要将Line Style Scale的属性设置为Annotation Scale)
- Patterns(Connect Edition的new feature)


### 设置Annotation Scale
Annotation Scale是Model的一个属性。所有放置于该Model中的Annotation都将遵循这里的设置的比例尺（虽然可以对某些元素进行单独不同比例尺设置，但是并不建议这么去做，容易造成混乱）

可以在Model属性窗口或者是Drawing Scale窗口进行设置。这两个窗口的设置值是连动的。

![](/img/2022/2022-10-15-11-16-20.png)

![](/img/2022/2022-10-15-11-16-33.png)

### Annotation Scale的用途
简单地说，Annotation Scale能够在两方面帮助您。

Annotation Scale能够控制您的Annotation比如文字标注，尺寸标注等的比例尺。比如，如果将一个3mm高的文字元素以1:10的Annotation Scale进行放置，在图纸上的高度将会表现为30mm。当您要对Model里所有的Annotation进行统一的修改的时候，您只需要更改Model的Annotation Scale。更改将自动对Model里的所有Annotation起作用。

在Sheet Model里，Annotation Scale影响的图纸的大小，图纸如下图的黑框所示。如果图纸大小选择A4，然后以Annotation Scale=1:1的大小放置，图纸边框大小正好是210mm×297mm。

![](/img/2022/2022-10-15-11-16-44.png)

这样就可以允许在图纸里插入比较大的Drawing模型。再比如，图纸大小还是选择A4，但是Annotation Scale选择1:1000，那么图纸就可以容纳210m×297m的模型（这里没有考虑图纸的边框）。所以，总的来说，Sheet是用来将模型表现在图纸上的，它有两个重要属性：Size和Annotation Scale，这两个数值的乘积就是他能表达的真正模型的范围，超出这个范围的模型将不能够被表现在图纸上。

如果图纸的边框是参考的另一个文件，并且在Sheet属性里的Border Attachment里关联了相应的边框，那么通过Annotation Scale放大缩小图纸的时候，图纸的边框也同样会被Annotation Scale影响。

![](/img/2022/2022-10-15-11-16-55.png)

![](/img/2022/2022-10-15-11-17-06.png)
 
### Annotation Scale一览
我们所看到的Annotation Scale一览其实是定义在Scales.def文件里的。

在Connect Edition里，该文件位于 C:\Program Files\Bentley\MicroStation CONNECT Edition\MicroStation\Default\Data 路径下。

在V8i里，该文件位于 C:\ProgramData\Bentley\MicroStation V8i (SELECTseries)\WorkSpace\System\data 路径下。

>因为旧版本Workspace里的System文件夹包含了很多重要文件，当根据某些需要CAD管理员将整个Workspace文件夹移动到网络服务器上，对System文件夹或者其中的文件进行了修改时，往往容易引起问题的发生。所以在Connect Edition里被移动到了MicroStation的安装目录下。
{: .prompt-info}

### Annotation Scale有用的Key-in命令
- LOCK USEANNOTATIONSCALE : 打开/关闭 Annotation Scale 开关
- ANNOTATIONSCALE ADD ：将没有使用Annotation Scale的元素（文字标注，尺寸标注等）加入Annotation Scale属性
- ANNOTATIONSCALE REMOVE：和上面的命令相反，将使用Annotation Scale的元素（文字标注，尺寸标注等）删除Annotation Scale属性
- MODEL SET ANNOTATIONSCALE：在后面加上具体的比例尺，能够改变模型的Annotation Scale设置值
- ANNOTATIONSCALE CHANGE：允许改变元素的Annotation Scale设置值，刚才也提及过，不建议单独改变某个元素Annotation设置值得做法，容易引起管理混乱

### 参考里的Annotation Scale
在V8i版本之前，是不可以将当前model的Annotation Scale适用到参考里的。您只能够activate参考模型，然后调整Annotation，最后再deactivate参考模型。过程比较繁琐。V8i版本之后可以让参考里的Annotation同样继承model里的Annotation Scale。这样，同一张图纸里的Annotation都可以使用相同的比例尺，而不用区分他们是属于sheet本身还是处于参考文件上。

![](/img/2022/2022-10-15-11-18-07.png)