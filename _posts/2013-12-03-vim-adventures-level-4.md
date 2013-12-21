---
layout: post
title: "VIM Adventures Level 4"
description: ""
category: "Vim"
tags: [VIM Adventures]
---
{% include JB/setup %}

##大单词(WORD) 与小单词(word)

我们现在来对比一下VIM中的大单词(WORD)与小单词(word)

* 小单词(word): 由字母、数字、下划线构成的连续字串，通过`w、e、b`在单词间跳转。
* 大单词(WORD): 简单地说就是以空白字符分割的单词，通过`W、E、B`在单词间跳转。

##替换命令r
               
* r 替换光标当前位置所在字符

第四关比较简单，首先通过`x`删除地图上红色的字符，然后学会`W、E、r`技能。使用`W，E`来在以空白字符分割的大单词(WORD)间移动光标，通过`r`修正红色字符。

