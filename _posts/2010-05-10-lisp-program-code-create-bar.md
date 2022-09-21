---
title: 绘横断面图标尺AutoLISP程序源代码
author: QinDong
date: 2010-05-10 08:56:10 +0800
categories: 编程开发 AutoLISP
tags: 横断面
excerpt: 
---
* content
{:toc}

本段代码是 [AutoCAD工程测量工具集](https://www.cehui.ren/posts/acad-toolkit/) 中的一个功能的源代码，可以单独运行。功能绘横断面图标尺。

```lisp
;;绘横断面高程标尺----------------------------------------------------------------------------------;;
(defun c:zzBC  (/ i judge varBarPosition varOrigin varBarPosition_tmp varTextHeight
		varBarPositionLevel varLength_tmp varLength varBarStartLevel varBarEndPositionLevel
		varBarEndLevel varBarStartX varBarStartY varBarsCount varStartY	varEndX	varEndY
		varPts)
  (setq i 0)
  (setq judge 0)
  (grtext -1 "绘制标尺 覃东")
  (princ "\n")
  (setq varOrigin (getpoint "请输入一点："))
  (princ "\n")
  (if (/= varLevel nil)
    (progn (setq varLevel_tmp varLevel)
	   (setq PromptTmp
		  (strcat "该点高程" "<" (rtos varLevel 2 3) ">:"))
	   )
    (setq PromptTmp "该点高程：")
    )
  ;;(while (<= (setq varLevel (getreal PromptTmp)) 0))
  (if (= (setq varLevel (getreal PromptTmp)) nil)
    (setq varLevel varLevel_tmp)
    )
  (princ "\n")
  (if (/= varBarPosition_tmp nil)
    (setq PromptTmp (strcat "标尺距离"
			    "<"
			    (rtos (car varBarPosition_tmp) 2 3)
			    ","
			    (rtos (cadr varBarPosition_tmp) 2 3)
			    ">:"
			    )
	  )
    (setq PromptTmp "标尺距离：")
    )
  (princ "\n")
  (if (= (setq varBarPosition (getpoint PromptTmp)) nil)
    (setq varBarPosition varBarPosition_tmp)
    )
  (setq varBarPosition_tmp varBarPosition)
  ;;选择标尺的顶点
  (if (/= varLength_tmp nil)
    (setq PromptTmp (strcat "标尺长度"
			    "<"
			    (rtos (car varLength_tmp) 2 3)
			    ","
			    (rtos (cadr varLength_tmp) 2 3)
			    ">:"
			    )
	  )
    (setq PromptTmp "标尺长度：")
    )
  (princ "\n")
  (if (= (setq varLength (getpoint PromptTmp)) nil)
    (setq varLength varLength_tmp)
    )
  (setq varLength_tmp varLength)

  (princ "\n")
  (if (/= varScale_tmp nil)
    (setq
      PromptTmp	(strcat	"比例尺分母"
			"<"
			(rtos varScale_tmp 2 3)
			">:")
      )
    (setq PromptTmp "比例尺分母：")
    )
  (if (= (setq varScale (getint PromptTmp)) nil)
    (setq varScale varScale_tmp)
    )

  (if (and (/= nil varScale)
	   (/= nil varScale)
	   (/= nil varBarPosition)
	   (/= nil varLength)
	   (/= nil varOrigin)
	   )
    ;;if main
    (progn
      ;;progn main

      ;;当确定标尺长度时若点在下方进行改正
      (if (<= (cadr varLength) (cadr varBarPosition))
	(setq varLength
	       (list (car varLength)
		     (+	(cadr varBarPosition)
			(abs (- (cadr varLength) (cadr varBarPosition)))
			)
		     )
	      )

	)

      (setq varScale_tmp varScale)
      (setq varScale (/ varScale 100))
      ;;比例下每cm长度
      (setq varTextHeight (* (/ varScale 10.0) 1.5))
      ;;文字高度及宽度为1.5mm
      (EntMakeTextStyle
	"LevelBar" varTextHeight 1 "simhei.ttf"	"")
      (EntMakeLayer "2-断面-标尺" 1)
      ;;确定标尺起点高程
      (setq varBarPositionLevel
	     (+	(- (cadr varBarPosition) (cadr varOrigin))
		varLevel
		)
	    )
      (setq varBarStartLevel (fix (+ varBarPositionLevel 0.5)))
      ;;四舍五入求标尺起点整高程
      ;;确定标尺终点高程
      (setq varBarEndPositionLevel
	     (+	(- (cadr varLength) (cadr varOrigin))
		varLevel
		)
	    )
      (setq varBarEndLevel (fix (+ varBarEndPositionLevel 0.5)))
      ;;四舍五入求标尺起点整高程
      ;;确定标尺起点坐标
      (setq varBarStartX (car varBarPosition))
      (setq varBarStartY
	     (+	(cadr varBarPosition)
		(- varBarStartLevel varBarPositionLevel)
		)
	    )
      (setq varBarsCount
	     (+	(atoi
		  (rtos
		    (/ (- varBarEndLevel varBarStartLevel) varScale)
		    2
		    0
		    )
		  )
		1
		)
	    )

      (setq varBarsCount (* (fix (+ (/ varBarsCount 2) 0.5)) 2))

      (while (/= varBarsCount 0)
	(setq varStartY (+ varBarStartY (* i varScale)))

	(setq varEndX (- varBarStartX (* (/ varScale 10.0) 1.5)))
	(setq varEndY (+ varBarStartY (* (+ i 1) varScale)))


	(setq Fp (list varBarStartX varStartY))
	(setq Ep (list varEndX varEndY))

	(setq Lfp (list varBarStartX varStartY))
	(setq Lep
	       (list (+ varBarStartX (/ varScale 10.0)) varStartY))

	(setq
	  Txtp
	   (list (+ varBarStartX (* (/ varScale 10.0) 2.0)) varEndY)
	  )
	(setq Hi (+ varBarStartLevel (* varScale i)))
	(setq
	  Loe (list (+ (/ varScale 10.0) varBarStartX)
		    varBarStartY)
	  )
	(if (= judge 0)
	  (progn
	    (setq SolidBarFp
		   (list (/ (+ (car Fp) (car Ep)) 2) (cadr Fp))
		  )
	    ;;实心标尺起点
	    (setq SolidBarEp
		   (list (/ (+ (car Fp) (car Ep)) 2) (cadr Ep))
		  )
	    ;;实心标尺终点
	    (entMakePLineThick
	      (list SolidBarFp SolidBarEp)
	      varTextHeight
	      "2-断面-标尺"
	      )
	    (EntMakeLine
	      (car lfp)
	      (cadr Lfp)
	      (car Lep)
	      (cadr Lep)
	      "2-断面-标尺"
	      )
	    (EntMakeText
	      (+ varBarStartX (* (/ varScale 10.0) 1.1))
	      varStartY
	      (itoa Hi)
	      varTextHeight
	      "LevelBar"
	      "2-断面-标尺"
	      )
	    (setq judge 1)
	    )
	  (progn
	    (setq varPts nil)
	    (setq varPts
		   (cons (list varBarStartX varStartY) varPts))
	    (setq
	      varPts (cons (list varBarStartX varEndY) varPts))
	    (setq varPts (cons (list varEndX varEndY) varPts))
	    (setq varPts (cons (list varEndX varStartY) varPts))
	    (entMakePLine varPts "2-断面-标尺")
	    (setq judge 0)
	    )
	  )
	(setq i (+ i 1))
	(setq varBarsCount (- varBarsCount 1))
	)
      (if (= judge 0)
	(progn
	  (setq Lfp (list varBarStartX varEndY))
	  (setq	Lep
		 (list (+ varBarStartX (/ varScale 10.0)) varEndY))
	  (setq Hi (+ varBarStartLevel (* varScale i)))
	  (EntMakeLine
	    (car lfp)
	    (cadr Lfp)
	    (car Lep)
	    (cadr Lep)
	    "2-断面-标尺"
	    )
	  (EntMakeText
	    (+ varBarStartX (* (/ varScale 10.0) 1.1))
	    varEndY
	    (itoa Hi)
	    varTextHeight
	    "LevelBar"
	    "2-断面-标尺"
	    )
	  )
	)
      (princ
	"\n(C)水电十二局宁海施工局测量队 覃东 qd.xyz@139.com"
	)
      (vl-cmdf "regen")
      (princ)
      )
    ;;end progn main
    (progn
      (princ
	"\n输入错误！请按提示输入！ (C)水电十二局宁海施工局测量队 覃东 qd.xyz@139.com"
	)
      (princ)
      )
    )
  ;;end if main

  )

;;绘横断面高程标尺----------------------------------------------------------------------------------;;
```
{: file='绘横断面标尺.lsp'}