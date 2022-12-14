---
layout: post
title:  "浅谈正则表达式中的分组和引用"
date:   2016-05-06 11:40:18 +0800
categories: 编程开发 语言基础
tags: 正则表达式
---

* content
{:toc}

由正则表达式如何匹配相同字符出发，讲讲正则表达式中的选择、分组和引用。

## 问题

在外刊君读者群中看到有人提出这样的一个需求：

> 把字符串切成连续相同字符的正则怎么写？比如`abbcccdddd`切成`a,bb,ccc,dddd`

之前我对正则表达式也是略有研究，想尝试一下。其实我对正则表达式的学习基本完全来源于犀牛书的第10章，真正看懂这一章，我觉得操作正则表达式应该不在话下。





## 我的答案

先给出我的答案吧：

```js
'abbccddd'.match(/(\w)\1*/g) // ["a", "bb", "cc", "ddd"]
```

## 说明


拿到这个问题，首先要匹配字符`[a-zA-Z0-9]`，这里直接使用`\w`。然后是全局匹配，在最后加上`g`。难点在于怎么判断重复。

翻看了犀牛书后，又读了一遍分组和引用的部分。使用小括号`()`将字符作为一个最小单元，同时小括号还能记忆这个组合相匹配的字符串。再使用反斜杠`\`引用前面分组的表达式，数字1表示第一个小括号。这时完成了2个字符重复的要求。最后再使用`*`来匹配出现0次或n次。这个正则表达式就写完了。

下面详细说说分组和引用。

**正则表达式的选择、分组和引用字符表**

字符 | 含义
--- | ---
`|` | 选择，匹配的是该符号左边的子表达式或右边的子表达式
`(...)` | 组合，将几个项组合为一个单元，这个单元可通过`*` `+` `?` `|` 等符号加以修饰，**而且可以记住和这个组合相匹配的字符串以提供伺候的引用使用**
`(?:...)` | 只组合，把项组合到一个段元，但不记忆与该组相匹配的字符
`\n` | 和第n个分组第一次匹配的字符相匹配，组是圆括号中的子表达式（也有可能是嵌套的），组索引是从左到右的左括号数，`(?:`形式的分组不编码
