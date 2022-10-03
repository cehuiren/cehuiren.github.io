---
title: 绘坐标方格网AutoLISP程序代码
author: QinDong
date: 2010-03-07 13:49:10 +0800
categories: 编程开发 AutoLISP
tags: 坐标 代码
excerpt: 
---
* content
{:toc}

绘坐标方格网程序代码,本段代码是 [AutoCAD工程测量工具集](https://www.cehui.ren/posts/acad-toolkit/) 中的一个功能的源代码，可以单独运行。

```lisp
;;生成方格网数据文件------------------------------------------------------------;;
;;;求模
(defun mod  (numA numB)
  (- (/ (* 1.0 numA) (* numB 1.0))
     (fix (/ (* numA 1.0) (* 1.0 numB)))
     )
  )

(defun C:zzFGW	(/ mscale gridDistCm pointId datFile pointA pointB pA pB gridDist zx_x zx_y zs_x
		 zs_y fileId gridPointX	gridPointY)
  (princ
    "功能：生成地形图方格网数据文件。(C)水电十二局宁海施工局测量队 覃东 2017.05 qd.xyz@139.com\n"
    )

  (setq	mscale
	 (getint "请输入成图比例尺分母(100/200/500/1000/2000/5000)："
		 )
	)
  (while (or (/= (mod mscale 100) 0) (<= mscale 0))
    (setq mscale
	   (getint "请输入成图比例尺分母100/200/500/1000/2000/5000：")
	  )
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
;;生成方格网数据文件------------------------------------------------------------;;
```
{: file='生成坐标方格网数据文件.LSP'}