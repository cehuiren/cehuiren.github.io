---
title: MicroStation精确绘图快捷键入命令的完整列表
author: QinDong
date: 2022-09-28 13:50:00 +0800
categories: MicroStation 基础
tags: AccuDraw 命令 快捷键
excerpt: 
---
* content
{:toc}

下表列出了每个键盘快捷键、对应的键入命令及其效果。有关各个键盘快捷键的效果的更多信息，请参阅精确绘图过程的一般讨论。这些快捷键仅在精确绘图窗口具有焦点时有效。 

键 | 键入命令 | 作用
--- | --- | ---
\<Enter\> | ACCUDRAW LOCK SMART | 智能锁 - 在直角坐标中，如果指针在绘图平面的 y 轴，则将 X 锁定到 0；如果指针在 x 轴，则将 Y 锁定到 0。在“极坐标”中，将“角度”锁定为 0°、90°、-90° 或 180°（如果指针位于绘图平面轴），或将“距离”锁定为最后输入的值。
\<M\> | ACCUDRAW MODE | 在“直角坐标”和“极坐标”之间切换。
\<O\> | ACCUDRAW SETORIGIN | 将绘图平面原点移动到当前指针位置。
\<V\> | ACCUDRAW ROTATE VIEW | 旋转绘图平面以与视图轴对齐。再次按下此键可恢复上下文相关旋转。
\<T\> | ACCUDRAW ROTATE TOP | 旋转绘图平面以与标准顶视图中的轴对齐。再次按下此键可恢复上下文相关旋转。
\<F\> | ACCUDRAW ROTATE FRONT | 旋转绘图平面以与标准前视图中的轴对齐。再次按下此键可恢复上下文相关旋转。
\<S\> | ACCUDRAW ROTATE SIDE | 旋转绘图平面以与标准侧视图中的轴对齐。再次按下此键可恢复上下文相关旋转。
\<B\> | ACCUDRAW ROTATE BASE TOGGLE | 旋转绘图平面以与激活 ACS 对齐，如果在对话框中设置某个旋转，它将带您返回到该旋转。在新文件（尚未使用 ACS 的文件）中，它将会旋转视图。 | 
\<E\> | ACCUDRAW ROTATE CYCLE | 在三个主平面之间旋转：顶视图、前视图和侧视图（仅用于三维）。当原始平面为 ACS 或上下文旋转时，此功能同样有效，因此您不必使用 RX、RY 来旋转至 90 度平面。 | 
\<X\> | ACCUDRAW LOCK X | 切换 X 值的锁状态。
\<Y\> | ACCUDRAW LOCK Y | 切换 Y 值的锁状态。
\<Z\> | ACCUDRAW LOCK Z | 切换 Z 值的锁状态。
\<D\> | ACCUDRAW LOCK DISTANCE | 切换距离值的锁状态。
\<A\> | ACCUDRAW LOCK ANGLE | 切换角度值的锁状态。
\<L, I\> | ACCUDRAW LOCK INDEX | 锁定当前索引状态。如果未索引轴或距离，则禁用索引。如果索引了轴或距离，则将其锁定。效果是暂时的，它将持续到输入数据点或再次运行快捷键为止。如果您需要索引到一个轴而不索引到另一轴，或者输入非常靠近某一轴但不在该轴上的数据点，此功能将非常有用。 | 
\<L, P\> | ACCUDRAW LOCK GRIDPLANE | 切换 ACS 平面和 ACS 平面捕捉锁，以及所有视图的栅格视图特性。
\<L, A\> | LOCK ACS TOGGLE | 切换 ACS 平面锁。
\<L, S\> | LOCK SNAP CONSTRUCTION TOGGLE | 切换 ACS 平面捕捉锁。
\<L, Z\> | ACCUDRAW LOCK STICKYZ | 切换粘性 Z 轴锁，它与 ACS 平面捕捉锁结合使用来强制一系列捕捉点位于激活 ACS 的 XY 平面 (Z=0) 上。
\<R, Q\> | ACCUDRAW ROTATE QUICK | 用于快速地临时旋转绘图平面。此过程在“快速旋转”键盘快捷键中进行了介绍。
\<R, A\> | ACCUDRAW ROTATE ACS | 用于永久旋转绘图视图。由于它旋转当前 ACS，因此，此旋转在使用的工具退出后仍将处于激活状态。如果打开，工具设置“使用当前原点”将导致绘图平面原点用作 x 轴原点，从而消除了输入额外数据点的必要。当然，在许多情况下，人们希望能够在不同于绘图平面原点的其他位置定义 x 轴原点。 | 
\<R, C\> | ACCUDRAW ROTATE CURRENTACS | 将绘图平面旋转到当前 ACS。
\<R, E\> | ACCUDRAW ROTATE ELEMENT | 旋转绘图平面以匹配选定元素的方向。
\<R, V\> | ACCUDRAW ROTATE ORIENTVIEW | 旋转激活视图以匹配当前绘图平面。
\<R, X\> | ACCUDRAW ROTATE X | 绕 x 轴将绘图平面旋转 90 度。
\<R, Y\> | ACCUDRAW ROTATE Y | 绕 y 轴将绘图平面旋转 90 度。
\<R, Z\> | ACCUDRAW ROTATE Z | 绕 z 轴将绘图平面旋转 90°。
\<?\> | ACCUDRAW DIALOG SHORTCUTS | 打开“精确绘图快捷键”窗口。
\<~\> | ACCUDRAW BUMP TOOLSETTING | 更改工具设置对话框中的控制选项（快捷键为 ~，通常在 \<Esc\> 键正下方 — 无需按 \<Shift\> 键）。它会找到工具设置对话框中第一个启用的控制选项，该控制选项是一个选项菜单或复选框。如果是选项菜单，则选择下一个有效值。如果是复选框，则将其从关闭切换为打开，反之亦然。例如，如果您正在绘制智能线且焦点在“精确绘图”窗口中，则可按 \<~\> 键，这样智能线便会切换为弧，而无需将焦点从“精确绘图”窗口中移走。 | 
\<G, T\> | DIALOG TOOLSETTING | 将焦点移动到“工具设置”窗口。
\<G, K\> | DIALOG CMDBROWSE | 打开（或将焦点移动到）“键入命令”窗口
\<G, S\> | ACCUDRAW DIALOG SETTINGS | 打开（或将焦点移动到）“精确绘图设置”对话框
\<G, A\> | ACCUDRAW DIALOG GETACS | 打开“获得 ACS”对话框，该对话框用于选择保存的辅助坐标系。
\<W, A\> | ACCUDRAW DIALOG SAVE ACS | 打开“写入 ACS”对话框，该对话框用于将绘图平面对齐方式保存为 ACS。
\<P\> | POINT KEYIN SINGLE | 打开“数据点键入命令”对话框以输入单个数据点。
\<M\> | POINT KEYIN MULTIPLE | 打开“数据点键入命令”对话框以输入多个数据点。
\<I\> | SNAP INTERSECT | 激活捕捉交点模式。
\<N\> | SNAP NEAREST | 激活“最近点”捕捉模式。
\<C\> | SNAP CENTER | 激活捕捉中心点模式。
\<K\> | ACCUDRAW DIALOG SNAPDIVISOR | 打开“关键点捕捉等分数”对话框，该对话框用于为关键点捕捉设置捕捉等分数。
\<H, A\> | ACCUDRAW Toggle | 暂停当前工具操作的精确绘图。选择新工具或输入“重置”可重新启用精确绘图。
\<H, S\> | ACCUSNAP Toggle | 打开或关闭精确捕捉。
\<H, U\> | ACCUSNAP SUSPEND | 暂停当前工具操作的精确捕捉。选择新工具或输入“重置”可重新启用精确捕捉。
\<Q\> | ACCUDRAW QUIT | 停用精确绘图。

除精确绘图快捷键外，ACCUDRAW LOCK GRIDPLANE 键入命令还会映射到 F8 功能键。它的功能与 \<L、P\> 快捷键相同，用于切换 ACS 平面和 ACS 平面捕捉锁，以及所有视图的栅格视图特性。 

注释: 键盘快捷键不区分大小写。 