---
title: MicroStation直线曲线裁剪曲面
author: QinDong
date: 2022-09-29 20:32:00 +0800
categories: MicroStation 曲面
tags: 裁剪 曲面
excerpt: 
---
* content
{:toc}

当一个曲面范围超出一部分，而超出部分可以用直线划分时，就可用直线裁剪曲面，使用命令为Trim By Curves：

![](/img/2022/2022-09-29-20-38-48.png)

参数设置为法线方向：

![](/img/2022/2022-09-29-20-38-54.png)

选择曲面、直线，确定即可完成裁剪。

>注意：单根的SmartLine不能作为裁剪边，但SmartLine线串却可以：
{: .prompt-warning}

![](/img/2022/2022-09-30-09-13-08.png)
_单根的智能线不能作为裁剪边智能线串可以用于裁剪_

![](/img/2022/2022-09-30-09-15-53.png)

因此，在绘裁剪边时，用`Place SmartLine`绘两段以上的线串即可。