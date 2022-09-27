---
title: AutoLISP源代码展CASS地形点生成三维小球体检查超欠挖
author: QinDong
date: 2012-09-27 13:58:00 +0800
categories: 编程开发 AutoLisp
tags: CASS 地形 超欠挖
excerpt: 
---
* content
{:toc}

```lisp
(defun C:zzZd (/ ptName ptSign ptE ptN ptH) ;读取DAT数据文件生成小球体
  (setq fi (getfiled "选择数据文件" "F:\\" "dat" 0))
  (setq f (open fi "r"))
  (setq rIndex 0)
  (while (setq lineStr (read-line f))
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
  (close f)


  ;;表处理
  (setq rIndex 0)
  (while (setq tmpPtname (nth rIndex ptName))
    (setq tmpE (atof (nth rIndex ptE)))
    (setq tmpN (atof (nth rIndex ptN)))
    (setq tmpH (atof (nth rIndex ptH)))
    (setq tmpSign (nth rIndex ptSign))

    ;;======自定义点处理程序区=======
    (command "sphere" (list tmpE tmpN tmpH) 0.5)
    (princ)
    ;;======自定义点处理程序区=======
    (setq rIndex (+ rIndex 1))
  )
  ;;表处理

  (princ)
)
```