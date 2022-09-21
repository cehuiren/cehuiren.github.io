---
title: 删除封闭区域内地形点AutoLISP程序源代码
author: QinDong
date: 2010-05-08 13:57:10 +0800
categories: 编程开发 AutoLISP
tags: 地形图 测点
excerpt: 
---
* content
{:toc}

本段代码是 [AutoCAD工程测量工具集](https://www.cehui.ren/posts/acad-toolkit/) 中的一个功能的源代码，可以单独运行。功能为将一封闭区域内的地形点全部删除。

```lisp
;;区域外地形点生成文件，删除区域内地形点------------------------------------------------------------;;
(defun C:zzQywdx
		  (/ ptName ptSign ptE ptN ptH ptCount filename1 filename2)
  (princ
    "功能：删除指定区域内的点，取区域外的地形点。(C)水电十二局宁海施工局测量队 覃东 2017.05 qd.xyz@139.com\n"
    )
  (setq	filename1
	 (getfiled "提示字符串"
		   "D:\\"
		   "dat"
		   2
		   )
	)
  (if filename1
    (progn
      (setq
	filename2 (strcat (car (splitx filename1 ".dat"))
			  "-区域外提取-"
			  (date)
			  "-"
			  (time)
			  ".dat"
			  )
	)
      (setq regionObj (entsel))
      (setq f1 (open filename1 "r"))
      (if (and f1 regionObj)
	(progn
	  (setq f2 (open filename2 "w"))
	  (setq rIndex 0)
	  (setq ptCount 0)
	  (while (setq lineStr (read-line f1))
	    (setq lineStrs (splitX lineStr ","))
	    (setq ptName (cons (nth 0 lineStrs) ptName))
	    ;;将点编号或点名生成表
	    (setq ptE (cons (nth 2 lineStrs) ptE))
	    ;;将E生成表
	    (setq ptN (cons (nth 3 lineStrs) ptN))
	    ;;将N生成表
	    (setq ptH (cons (nth 4 lineStrs) ptH))
	    ;;将H生成表
	    (setq ptSign (cons (itoa rIndex) ptSign))
	    ;;生成记号表
	    (setq rIndex (+ rIndex 1))
	    )
	  (setq ptName (reverse ptName))
	  ;;反序
	  (setq ptSign (reverse ptSign))
	  ;;反序
	  (setq ptE (reverse ptE))
	  ;;反序
	  (setq ptN (reverse ptN))
	  ;;反序
	  (setq ptH (reverse ptH))
	  ;;反序
	  (close f1)
	  ;;表处理
	  (setq rIndex 0)
	  (while (setq tmpPtname (nth rIndex ptName))
	    (setq tmpE (nth rIndex ptE))
	    (setq tmpN (nth rIndex ptN))
	    (setq tmpH (nth rIndex ptH))
	    (setq tmpSign (nth rIndex ptSign))
	    (if
	      (not (pt_inorout
		     regionObj
		     (list (atof tmpE) (atof tmpN)))
		   )
	       (progn
		 (write-line
		   (strcat tmpPtname ",," tmpE "," tmpN "," tmpH)
		   f2
		   )
		 (setq ptCount (+ 1 ptCount))
		 )
	       )
	    (setq rIndex (+ rIndex 1))
	    )
	  ;;表处理
	  (princ
	    (strcat
	      "共提取"
	      (itoa ptCount)
	      "个点。(C)水电十二局宁海施工局测量队 覃东 2017.05 qd.xyz@139.com\n"
	      )
	    )
	  ;;(princ "\n")
	  (close f2)
	  )
	;;End progn
	(princ
	  "用户取消操作！(C)水电十二局宁海施工局测量队 覃东 2017.05 qd.xyz@139.com\n"
	  )
	)
      ;;end if

      )
    ;; end progn1
    (princ
      "用户取消操作！(C)水电十二局宁海施工局测量队 覃东 2017.05 qd.xyz@139.com\n"
      )
    )
  ;;end if1
  (princ)
  )
;;区域外地形点生成文件，删除区域内地形点------------------------------------------------------------;;
```
{: file='删除区域内地形点.lsp'}