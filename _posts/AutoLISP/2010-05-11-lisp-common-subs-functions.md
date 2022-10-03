---
title: AutoLISP自定义通用过程函数
author: QinDong
date: 2010-05-11 09:06:16 +0800
categories: 编程开发 AutoLISP
tags: 函数 通用代码
excerpt: 
---
* content
{:toc}

本段代码是 [AutoCAD工程测量工具集](https://www.cehui.ren/posts/acad-toolkit/) 中可能调用的自定义通用函数。

```lisp
;;自定义通用函数----------------------------------------------------------------------------------;;

;;用法：(EntMakeText 点X 点Y 文本内容 文本高度)
(defun EntMakeText  (px py str tHeight styleName layerName / pt)
  (setq pt (list px py))
  (entmakeX
    (list '(0 . "TEXT")
	  (cons 1 str)
	  (cons 10 pt)
	  (cons 7 styleName)
	  (cons 8 layerName)
	  (cons 40 tHeight)
	  )
    )
  )

;;用法：(EntMakeLine 起点X 起点Y 终点X 终点Y)
(defun EntMakeLine  (xa ya xb yb layerName / p1 p2)
  (setq	p1 (list xa ya)
	p2 (list xb yb)
	)
  (entmakeX (list '(0 . "LINE")
		  '(370 . 0)
		  (cons 10 p1)
		  (cons 11 p2)
		  (cons 8 layerName)
		  )
	    )
  )

(defun entMakePLineThick  (pts weight layerName)
  (entmake (append
	     (list '(0 . "LWPOLYLINE")
		   '(100 . "AcDbEntity")
		   '(100 . "AcDbPolyline")
		   (cons 8 layerName)
		   ;;层
		   ;;'(62 . 7)
		   ;;颜色：7-白色
		   '
		    (370 . 0)
		   ;;线宽0，0.20值为(370 . 20)
		   (cons 90 (length pts))
		   (cons 43 weight)
		   ;;全局宽
		   '
		    (70 . 0)
		   ) ;list
	     (mapcar '(lambda (x)
			(cons 40 weight)
			;;起点宽
			(cons 41 weight)
			;;终点宽
			(cons 42 0.0)
			(cons 10 x)
			)
		     pts
		     )
	     ) ;append
	   )
  )


(defun entMakePLine  (pts layerName)
  (entmake (append
	     (list '(0 . "LWPOLYLINE")
		   '(100 . "AcDbEntity")
		   '(100 . "AcDbPolyline")
		   '(370 . 0)
		   (cons 8 layerName)
		   (cons 90 (length pts))
		   '(70 . 0)
		   ) ;list
	     (mapcar '(lambda (x) (cons 10 x)) pts)
	     ) ;append
	   )
  )

(defun EntMakeTextStyle	 (tStyleName tStyleHeight tStyleWeight tFontName tBigFontName)
  (if (not (tblsearch "style" tStyleName))
    (entmakeX
      (list
	'(0 . "STYLE")
	'(100 . "AcDbSymbolTableRecord")
	'(100 . "AcDbTextStyleTableRecord")
	(cons 2 tStyleName)
	'(70 . 0)
	(cons 40 tStyleHeight)
	(cons 41 tStyleWeight)
	(cons 3 tFontName)
	(cons 4 tBigFontName)
	)
      )
    )
  )

(defun EntMakeLayer  (layname color / nlay)
  (vl-load-com)
  (or (tblsearch "layer" layname)
      (or (not (setq nlay
		      (vla-add (vla-get-layers
				 (vla-get-activedocument (vlax-get-acad-object))
				 )
			       layname
			       )
		     )
	       )
	  (vla-put-color nlay color) ;vla-put-返回值为nil
	  ;(vla-put-plottable nlay :vlax-false) ;设为不打印层
	  ;(vla-put-activelayer
	  ;  (vla-get-activedocument (vlax-get-acad-object))
	  ;  nlay
	  ;)
	  ;;设为当前层
	  )
      )
  )


;;;功能：字符串按指定分隔符分隔,分隔符可以是字串，用于取文件名，用扩展名作为分隔符
;;;(splitX "C:\\Users\\....25～K1+013.52；EL.2776.73～EL.2804.74）.dat" ".dat")
;;;返回：(C:\\Users\\....25～K1+013.52；EL.2776.73～EL.2804.74）)
(defun splitX  (str delim / LST POS)
  (while (setq pos (vl-string-search delim str))
    (setq lst (append lst (list (substr str 1 pos))))
    (setq str (substr str (+ (+ pos (strlen delim)) 1)))
    )
  (if (> (strlen str) 0)
    (append lst (list str))
    lst
    )
  )


(defun pt_inorout  (regionObj pt / pt_list e1 pt n i j va va_count)
  (setq	pt_list	(mapcar	'cdr
			(vl-remove-if
			  '(lambda (x) (/= 10 (car x)))
			  (entget (car regionObj))
			  )
			)
	)

  (setq	i	 0
	va_count 0
	n	 (length pt_list)
	pt_list	 (append pt_list (list (car pt_list)))
	)
  (repeat n
    (setq va (-	(angle pt (nth i pt_list))
		(angle pt (nth (1+ i) pt_list))
		)
	  )
    (cond ((> va pi) (setq va (- va pi)))
	  ((< va (* -1 pi)) (setq va (+ va pi)))
	  )
    (setq va_count (+ va_count va)
	  i	   (1+ i)
	  )
    )
  (if (< (abs (- (abs va_count) pi)) 0.000001)
    't
    'nil
    )
  )


(defun date  ()
  (setq datetime (rtos (getvar "cdate") 2 6))
  (car (splitx datetime "."))
  )

(defun time  ()
  (setq datetime (rtos (getvar "cdate") 2 6))
  (cadr (splitx datetime "."))
  )
```
{: file='自定义通用函数.lsp'}


```lisp
(defun C:zzHelp	 ()
  (princ
    "\n-测量工具箱 | Ver:20200817 | 覃东 水电十二局宁海施工局测量队-"
    )
  (princ "\n01 zzFGW：生成方格网坐标数据文件(D:盘)")
  (princ "\n02 zzArea2Table：输出面积到AutoCAD表格")
  (princ "\n03 zzArea2File：输出面积到文件")
  (princ "\n04 zzA：标注面积到区域中心")
  (princ "\n05 zzQydx：区域地形：删除指定封闭区域外的地形点")
  (princ
    "\n06 zzQywdx：区域外地形：删除指定封闭区域内的地形点"
    )
  (princ "\n07 zzBC：横断面高程标尺：绘制横断面图的高程标尺")
  (princ
    "\n08 zzExport2Dat：CASS地形图导出无编码数据文件(.dat)"
    )
  (princ
    "\n09 zzCass2CadPoint：CASS地形图生成CAD点对象用于生成C3D曲面"
    )
  (princ "\n10 zzHelp：查看命令帮助提示信息")
  (princ "\n(c)QinDong qd.xyz@139.com 2020-08-17 覃东 宁海")
  (princ)
  )

(vl-load-com)
(princ)
(princ
  "\n-测量工具箱 | Ver:20200817 | 覃东 水电十二局宁海施工局测量队-"
  )
(princ "\n01 zzFGW：生成方格网坐标数据文件(D:盘)")

(princ "\n05 zzQydx：区域地形：删除指定封闭区域外的地形点")
(princ
  "\n06 zzQywdx：区域外地形：删除指定封闭区域内的地形点"
  )
(princ "\n07 zzBC：横断面高程标尺：绘制横断面图的高程标尺")
(princ
  "\n08 zzExport2Dat：CASS地形图导出无编码数据文件(.dat)"
  )
(princ
  "\n09 zzCass2CadPoint：CASS地形图生成CAD点对象用于生成C3D曲面"
  )
(princ "\n10 zzHelp：查看命令帮助提示信息")
(princ "\n(c)QinDong qd.xyz@139.com 2020-08-17 覃东 宁海")
(princ)

;;------------------------------------------------------------;;
;;                         End of File                        ;;
;;------------------------------------------------------------;;
```
{: file='简短帮助.lsp'}