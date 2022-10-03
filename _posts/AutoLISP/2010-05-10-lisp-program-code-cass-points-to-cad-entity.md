---
title: CASS地形图地形点单元转换为AutoCAD实体点对象
author: QinDong
date: 2010-05-10 09:56:10 +0800
categories: 编程开发 AutoLISP
tags: 地形图 测点
excerpt: 
---
* content
{:toc}

本段代码是 [AutoCAD工程测量工具集](https://www.cehui.ren/posts/acad-toolkit/) 中的一个功能的源代码，可以单独运行。功能为将CASS地形图地形点的单元对象转换为CAD点实体对象。

```lisp
;;CASS地形图生成CAD点对象用于生成C3D曲面--------------------------------------------------------------------;;
;;2019-06-01 (C) QinDong
(defun C:zzCass2CadPoint  ()
	  ;(setq ss (ssget "X" '((0 . "insert") (8 . "GCD"))))
  (setq ss (ssget "X" '((0 . "insert") (2 . "GC*"))))

  (setq i 0)
  (repeat (sslength ss)
    (setq ssn (ssname ss i))
    (setq endata (entget ssn))
    (setq blockXYZ (assoc 10 endata))
    (print blockXYZ)
    (entmakeX (list '(0 . "POINT") blockXYZ))
    (setq i (1+ i))
    (entdel ssn)
    )

  )
```
{: file='CASS地形点转换为CAD点实体.lsp'}