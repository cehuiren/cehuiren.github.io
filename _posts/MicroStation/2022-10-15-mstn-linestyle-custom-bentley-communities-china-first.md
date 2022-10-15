---
title: MicroStation如何自定义线型
author: 中国优先社区
date: 2022-10-15 10:51:00 +0800
categories: MicroStation 基础
tags: 线型
excerpt: 
---
* content
{:toc}

简单来说，自定义线型主要包括三个部分的自定义：stroke pattern、point symbol以及compound definition(其实也就是Stroke Pattern和Point Symbol的集合)。
 
![](/img/2022/2022-10-15-10-43-17.png)

在MicroStation里自定义线型只需要下面六步。接下来以MicroStation CE举例说明。

- 打开线型编辑器.
- 新建或者打开一个既存的线型库文件.
- 新建线型对象.
- 新建stroke pattern(s) 以及 point component(s).
- 新建compound definition.
- 关联新建的线型对象和和compound definition.
- 下面将对每个步骤详细说明。

1、通过File⇒Settings⇒File⇒Line Style Editor打开线型编辑器。

![](/img/2022/2022-10-15-10-43-37.png)
 
![](/img/2022/2022-10-15-10-43-51.png)

2、这里我们可以从File菜单里新建或者修改一个既存的线型库文件。当新建一个线型库文件的时候，默认包含8种内部线型。请看下面的截图。

![](/img/2022/2022-10-15-10-44-05.png)

3、Edit⇒Create⇒Name新建线型，可以根据需要重命名。

![](/img/2022/2022-10-15-10-44-17.png)

4、新建Stroke Pattern。打开Edit⇒Create⇒Stroke Pattern。可以根据需要重命名。

![](/img/2022/2022-10-15-10-44-31.png)

接下来我们需要定义新线型的dashes和gaps。

我们的例子里新线型有3个dashes，因此点击Add按钮添加三段。

![](/img/2022/2022-10-15-10-44-41.png)

每个线段是可以被选择的，点击各个线段可以单独进行设置。在这里我们做如下设置：Fixed Length: 1.0、Stroke Type: Dash。其他的设置我们保持默认值。

![](/img/2022/2022-10-15-10-44-50.png)

中间的线段的设置如下： Fixed Length: 0.5   Stroke Type: Dash.

最末端线段的设置如下： Fixed Length: 1.0   Stroke Type: Dash.

然后，我们还可以通过Edit⇒Create⇒Point新建Point，并且根据需要重命名。

![](/img/2022/2022-10-15-10-45-01.png)

点击Base Stroke Pattern按钮关联Base Stroke Pattern。

![](/img/2022/2022-10-15-10-45-12.png)

下面我们将使用MicroStation的画图工具创建一个三角形的符号作为Point对象。我们将底边设置为0.5，刚好和中间线段的长度相同，我们的目的就是为了将三角形符号放在中间线段的位置。

使用围栏工具框选住创建好的三角形符号或者使用select工具选中它。点击Create按钮（注意，只有当使用围栏工具或者select工具时，Create按钮才可用），输入名称，选中插入点，三角形符号就被加入了point库中。

![](/img/2022/2022-10-15-10-45-33.png)
 
![](/img/2022/2022-10-15-10-45-46.png)

![](/img/2022/2022-10-15-10-45-57.png)

>**小技巧1**：如果要加入一个既存的point对象，可以使用keyin：place symbol [name].  (ie:  place symbol fault)
{: .prompt-info}

>**小技巧2**：如果要从point库里删除一个既存的对象，可以使用keyin：delete symbol [name]  (ie:  delete symbol fault)
{: .prompt-info}

>**小技巧3**：同时按下CTRL+SHIFT点击鼠标左键可以使用accusnap确定插入点
{: .prompt-info}

然后在选择好插入线段的基础上，点击select指定point图符。

![](/img/2022/2022-10-15-10-46-08.png) 

设置好的结果会动态更新。

![](/img/2022/2022-10-15-10-46-18.png)

5、通过Edit⇒Create⇒Compound新建Compound对象，点击insert插入刚才新建好的Point和Stroke。

![](/img/2022/2022-10-15-10-46-31.png)

6、最后选择好LineStyle以及Compound，点击Edit⇒Link，关联。操作结束后，会在Compound的前面出现>>这样的符号。完毕。

![](/img/2022/2022-10-15-10-46-47.png)

接下来您就可以使用自己的linestyle了。

![](/img/2022/2022-10-15-10-46-57.png)

[原文出处。](https://communities.bentley.com/communities/other_communities/chinafirst/w/chinawiki/28700/28700)