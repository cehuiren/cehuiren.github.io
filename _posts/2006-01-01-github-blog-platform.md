---
layout: post
title:  "Visual Studio Code blog platform"
date: 2006-01-01
categories: 系统软件 网络
tags: blog
excerpt: 在Visual Studio Code上搭建github博客管理平台
author: QinDong
---
* content
{:toc}

### VSC Blog IDE
1. [安装Visual Studio Code](https://code.visualstudio.com/)，也可在Windows应用商店中安装；
2. [安装用于VSC的git](https://code.visualstudio.com/docs/editor/versioncontrol)
3. [安装Windows桌面版git](https://git-scm.com/downloads/guis)
4. [安装git](https://git-scm.com/download/win)
    三个git均可同时安装，可在VSC内提交、也可在桌面版的git内提交
5. 使用桌面版git克隆blog；
6. **在VSC内将blog目录qin-dong.github.io设置为工作区主目录**；
7. [安装VSC扩展PasteImage，并进行相应设置](https://qin-dong.github.io/2022/01/01/blog-vscode-pasteimage-settings/)
8. 安装VSC扩展MarkDown All in One。


### PasteImage设置参数
```
Base Path:${projectRoot}
Default Name:Y-MM-DD-HH-mm-ss
Encode Path:rulEncodeSpace
File Path Confirm Imput Box Mode:onlyName
Force Unix Style Separator:Checked
'Inser Patterm:${imageSyntaxPrefix}${imageFilePath}${imageSyntaxSuffix}
'居中显示图像
Inser Patterm:<div style="text-align:center;"><img src="${imageFilePath}"></div>
Name Prefix:null
name Suffix:null
Path:${projectRoot}/img/2022/
Prefix:/
Show File Path Confirm Imput Box:unchecked
Suffix:null
```

### VSC扩展
- PasteImage
- Markdown Preview Enhanced
- Markdown PDF
- Highlight Matching Tag
- Todo Tree
- AutoCAD AutoLISP Extension