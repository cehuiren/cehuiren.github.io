---
title:  "Blog Theme Chirpy Upgrading"
date: 2006-01-01
categories: 系统软件 网络
tags: blog
excerpt: 博客主题Chirpy升级总结
author: QinDong
---
* content
{:toc}

Chirpy由Starter安装，按下列操作从5.2.1升级5.3.0成功。

### 升级说明

1. 版本比较：

进入[Chirpy库](https://github.com/cotes2020/jekyll-theme-chirpy)，进入[Wiki](https://github.com/cotes2020/jekyll-theme-chirpy/wiki)，查看[Upgrade Guide](https://github.com/cotes2020/jekyll-theme-chirpy/wiki/Upgrade-Guide)

2. [升级说明](https://chirpy.cotes.page/posts/getting-started/#upgrading)

>https://chirpy.cotes.page/posts/getting-started/#upgrading

### 文件升级

使用以下格式获得升级需要修改的文件列表及内容：

>https://github.com/cotes2020/chirpy-starter/compare/<older-version>...<newer-version>

如[从v4.0.0升级到v5.0.0](https://github.com/cotes2020/chirpy-starter/compare/v4.0.0...v5.0.0):

>https://github.com/cotes2020/chirpy-starter/compare/v4.0.0...v5.0.0

根据列表修改或添加文件。

### 升级命令

#### 在 Getting Started 介绍中有关升级的内容：

```
- gem "jekyll-theme-chirpy", "~> 3.2", ">= 3.2.1"
+ gem "jekyll-theme-chirpy", "~> 3.3", ">= 3.3.0"
```
{: file='Gemfile'}

1. 按下列格式修改 **Gemfile** (在版本比较中此文件修改内容也会罗列，如果已修改，可忽略)。

2. 运行更新命令

```
$ bundle update jekyll-theme-chirpy
```
#### 升级更新

在Github Pages环境运行命令只有在工作流中进行：
- 进入库；
- 进入actions；
- New workflow；
- set up a workflow yourself;
- 将命令`bundle update jekyll-theme-chirpy`添加到新建的yml文件中运行单个命令run：后。

提交后网页更新时会出现错误报警，但稍后却成功更新成功，以后升级时再仔细斟酌。