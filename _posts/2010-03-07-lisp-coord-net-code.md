---
title: 南方CASS点状地物符号代码展点程序
author: 网格转载
date: 2010-03-07 13:49:10 +0800
categories: 编程开发 AutoLisp
tags: CASS 代码
excerpt: 
---
* content
{:toc}

绘坐标方格网程序代码。

```lisp
;;;求模
(defun mod (numA numB)
  (- (/ (* 1.0 numA) (* numB 1.0))
     (fix (/ (* numA 1.0) (* 1.0 numB)))
  )
)

(defun C:zzFGW (/	  mscale    gridDistCm		pointId
		datFile	  pointA    pointB    pA	pB
		gridDist  zx_x	    zx_y      zs_x	zs_y
		fileId	  gridPointX	      gridPointY
	       )
;;;(initget 7 "100 200 500 1000 2000 5000")
  (setq mscale (getint "请输入成图比例尺分母(100/200/500/1000/2000/5000)："))
  (while (or (/= (mod mscale 100) 0) (<= mscale 0))
    (setq mscale (getint "请输入成图比例尺分母100/200/500/1000/2000/5000："))
  )

;;;(setq gridDistCm (getint "请输入格网距离(cm):"))
  (setq gridDistCm 10)
;;;默认10cm
  (setq pointId 0)
;;;(setq	datFile
;;; (getfiled "请输入格网坐标文件名："
;;;	   (cond (*file*)
;;;		 ("")
;;;	   )
;;;	   "dat"
;;;	   1
;;;)
;;;)
  (setq datFile "D:\\地形图坐标方格网数据.dat")

  (setq pointA (getpoint "请输入第一点："))
  (princ (strcat "N:"
		 (rtos (cadr pointA) 2 3)
		 " E:"
		 (rtos (car pointA) 2 3)
		 "\n"
	 )
  )
  (setq pointB (getpoint "请输入第二点："))
  (princ (strcat "N:"
		 (rtos (cadr pointB) 2 3)
		 " E:"
		 (rtos (car pointB) 2 3)
		 "\n"
	 )
  )
  (setq	pA pointA
	pB pointB
  )
  (setq	pointA (list (min (car pA) (car pB))
		     (min (cadr pA) (cadr pB))
	       )
  )

  (setq	pointB (list (max (car pA) (car pB))
		     (max (cadr pA) (cadr pB))
	       )
  )
;;;格网距离：米
  (setq gridDist (* (/ mscale 100) gridDistCm))
  (setq zx_x (* (+ (fix (/ (car pointA) gridDist)) 1) gridDist))
  (setq zx_y (* (+ (fix (/ (cadr pointA) gridDist)) 1) gridDist))

  (setq zs_x (* (fix (/ (car pointB) gridDist)) gridDist))
  (setq zs_y (* (fix (/ (cadr pointB) gridDist)) gridDist))

  (setq gridPointX zx_x)

  (setq fileId (open datFile "w"))
  (while (<= gridPointX zs_x)
    (setq gridPointY zx_y)
    (while (<= gridPointY zs_y)
      (setq pointId (+ pointId 1))
      (write-line
	(strcat	(itoa pointId)
		",+,"
		(itoa gridPointX)
		","
		(itoa gridPointY)
		",0.0"
	)
	fileId
      )
      (setq pointId (+ pointId 1))
      (write-line
	(strcat	(itoa pointId)
		",NE,"
		(itoa gridPointX)
		","
		(itoa gridPointY)
		",0.0"
	)
	fileId
      )

      '(command "_point" (list gridPointX gridPointY))
      (setq gridPointY (+ gridPointY gridDist))
    )
;;;end while y

    (setq gridPointX (+ gridPointX gridDist))
  )
;;;end while x

  (setq fileId (close fileId))
  (princ)
)
```
{: file='生成坐标方格网数据文件.LSP'}