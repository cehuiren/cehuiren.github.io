---
title: AutoLISP绘三维对象实体代码
author: QinDong
date: 2012-09-26 10:38:00 +0800
categories: 编程开发 AutoLisp
tags: 三维 实体
excerpt: 
---
* content
{:toc}

### 1、直接画线
```lisp
(command "line"  (list s1 s2 s3) (list s4 s5 s6) "" )
(command "line"  (list s7 s8 s9) (list s10 s11 s12) "" )
```

### 2、直接画面
```lisp
(command "3dface"  (list s1 s2 s3) (list s4 s5 s6) (list s7 s8 s9) (list s10 s11 s12)"" )
(command "3dface"  (list s13 s14 s15)(list s16 s17 s18)(list s19 s20 s21)(list s22 s23 s24)"" )
```

### 3、直接画体
```lisp
(command "3dpoly" (list s1 s2 s3) (list s4 s5 s6) (list s7 s8 s9)(list s1 s2 s3)"" )
(command "3dpoly"  (list s10 s11 s12)(list s13 s14 s15)(list s16 s17 s18)(list s10 s11 s12)"" )
(command "loft"  (entnext)(entlast)  "" "")
``` 