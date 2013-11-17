---
layout: post
title: "VIM Adventures Level 2"
description: ""
category: "Vim"
tags: [VIM Adventures]
---
{% include JB/setup %}

##Vim中的单词（word）

我们先来看看Vim中是如何定义一个单词(word)

* 由字母、数字、下划线构成的连续字串（如：`apple_vim123`）
* 其它非空字符（空格、换行、tab属于空字符）构成的连续字串（如: `$#@()+-`）
* 由中文字符构成的连续字串（如：`我喜欢苹果电脑`）


##单词移动

Level 1中我们学会了用hjkl来上下左右移动光标，下面我们来看看如何在单词间移动。

* w 光标移动到下一个单词首字符
* b 光标向前移动到单词首字符
* e 光标向后移动到单词末字符

如果你理解了`w,b,e`，那么第二关就是so easy了。
