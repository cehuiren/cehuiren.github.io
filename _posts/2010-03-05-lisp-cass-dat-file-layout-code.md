---
title: 南方CASS点状地物符号代码展点程序
author: 网格转载
date: 2010-03-05 13:39:10 +0800
categories: 编程开发 AutoLISP
tags: CASS 代码
excerpt: 
---
* content
{:toc}

网上看到的一个南方CASS点状地物符号代码展点程序，未验正正确与否。

```lisp
(defun c:dzw(/ file d key) ;主程序
  (setq file (open (getfiled "选择要数据文件<.dat>" "c:/" "dat;txt;*" 8) "r"))
  (princ "\n")
  (setvar "CMDECHO" 0)
  (command "undo" "be")
  (while 
  (setq d (read-line file))
  (setq key(substr d 1 2))
;以下为规律 每行一个类别 可以编辑
;              关键字                       块名                                 颜色、图层                       cass编码
   (cond
        ((and(= key "YS")(findblock (getid d) "gcbj0103"))(entmake-dzw "gcbj0103" (getid d) 8 "COMPONENT" '((-3 ("SOUTH" (1000 . "340103"))))));雨水＃
        ((and(= key "WS")(findblock (getid d) "gcbj0102"))(entmake-dzw "gcbj0102" (getid d) 8 "COMPONENT" '((-3 ("SOUTH" (1000 . "340102"))))));污水＃
        ((and(= key "DX")(findblock (getid d) "gcbj0107"))(entmake-dzw "gcbj0107" (getid d) 8 "COMPONENT" '((-3 ("SOUTH" (1000 . "340107"))))));电信＃
        ((and(= key "JS")(findblock (getid d) "gcbj0101"))(entmake-dzw "gcbj0101" (getid d) 8 "COMPONENT" '((-3 ("SOUTH" (1000 . "340101"))))));给水＃
        ((and(= key "GS")(findblock (getid d) "gcbj0101"))(entmake-dzw "gcbj0101" (getid d) 8 "COMPONENT" '((-3 ("SOUTH" (1000 . "340101"))))));给水＃
        ((and(= key "DL")(findblock (getid d) "gcbj0105"))(entmake-dzw "gcbj0105" (getid d) 8 "COMPONENT" '((-3 ("SOUTH" (1000 . "340105"))))));电力＃
        ((and(= key "BM")(findblock (getid d) "gcbj0114"))(entmake-dzw "gcbj0114" (getid d) 8 "COMPONENT" '((-3 ("SOUTH" (1000 . "340114"))))));不明＃
        ((and(= key "RQ")(findblock (getid d) "gcbj0111"))(entmake-dzw "gcbj0111" (getid d) 8 "COMPONENT" '((-3 ("SOUTH" (1000 . "340111"))))));燃气＃
        ((and(= key "JT")(findblock (getid d) "gcbj0112"))(entmake-dzw "gcbj0112" (getid d) 8 "COMPONENT" '((-3 ("SOUTH" (1000 . "340112"))))));交通＃
        ((and(= key "CH")(findblock (getid d) "gc298"))(entmake-dzw "gc298" (getid d) 8 "DLSS" '((-3 ("SOUTH" (1000 . "165631"))))))           ;车红
        ((and(= key "RH")(findblock (getid d) "gc299"))(entmake-dzw "gc299" (getid d) 8 "DLSS" '((-3 ("SOUTH" (1000 . "165632"))))))           ;人红
        ((and(= key "YB")(findblock (getid d) "gcbj0104"))(entmake-dzw "gcbj0104" (getid d) 8 "DLSS" '((-3 ("SOUTH" (1000 . "340104"))))))     ;雨篦
        ((and(= key "FN")(findblock (getid d) "gc111"))(entmake-dzw "gc111" (getid d) 24 "DLDW" '((-3 ("SOUTH" (1000 . "158703"))))))          ;坟
        ((and(= key "SE")(findblock (getid d) "gc019"))(entmake-dzw "gc019" (getid d) 7 "DLDW" '((-3 ("SOUTH" (1000 . "155221"))))))           ;射灯
        ((and(= key "LD")(findblock (getid d) "gcbj0118"))(entmake-dzw "gcbj0118" (getid d) 7 "COMPONENT" '((-3 ("SOUTH" (1000 . "340118"))))));路灯
        ((and(= key "LB")(findblock (getid d) "gc052"))(entmake-dzw "gc052" (getid d) 5 "DLSS" '((-3 ("SOUTH" (1000 . "165603"))))))           ;路标
        ((and(= key "JK")(findblock (getid d) "gcbj0612"))(entmake-dzw "gcbj0612" (getid d) 7 "COMPONENT" '((-3 ("SOUTH" (1000 . "340612"))))));监控
        ((and(= key "XF")(findblock (getid d) "gcbj0113"))(entmake-dzw "gcbj0113" (getid d) 8 "COMPONENT" '((-3 ("SOUTH" (1000 . "340113"))))));消防栓
        ((and(= key "FN")(findblock (getid d) "gc111"))(entmake-dzw "gc111" (getid d) 8 "DLDW" '((-3 ("SOUTH" (1000 . "158703"))))));坟
  );cond
  );while
  (command "undo" "e")
  (command "ZOOM" "E")
  (princ)
  )
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
(defun getid(d / ddd xok yok);获取坐标 (9.26541e+006 9.18972e+006 0)
  (while (vl-string-search " " d) (setq d (vl-string-subst "" " " d)));如果点号有空格，替换之
  (while (vl-string-search "," d) (setq d (vl-string-subst " " "," d)));用空格替换逗号
  (setq DDD (read (strcat "(" d ")")));以空格分隔的方式读取点号坐标高程
    (if (/= (NTH 4 DDD) nil);;;如果坐标表 是5列
    (progn;;;
      (SETQ xok (NTH 2 DDD))
      (SETQ yok (NTH 3 DDD))
      )
      (progn;;;如果坐标表 是4列
         (SETQ xok (NTH 1 DDD))
         (SETQ yok (NTH 2 DDD))
      )
)
  (list  xok yok )
  )
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
(defun entmake-dzw(km pt co la bm / data);插块 改颜色图层编码
    (command "_.insert" km "x" 0.5 "y" 0.5 "non" pt"")
    (command "chprop" (entlast)"" "c" co "la" la "")
    (setq data(entget(entlast)))
    (setq data(append data bm))
    (entmod data)
  )

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
(defun findblock(id block / p1 P2 P3 P4);   60厘米范围内找到同样的图块，就不绘制（可调）
  (command "zoom" "c" id 10)
  (setq P1(mapcar '+ id '(0.6 0.6)))
  (setq P2(mapcar '+ id '(0.6 -0.6)))
  (setq P3(mapcar '+ id '(-0.6 -0.6)))
  (setq P4(mapcar '+ id '(-0.6 0.6)))
  (if(null(ssget "_CP"  (LIST p1 p2 p3 p4)(list(cons 2 block))))
    T
   )
  )
  ```