---
layout: post
title: "VIM Adventures Level 3"
description: ""
category: "Vim"
tags: [VIM Adventures]
---
{% include JB/setup %}

##Vim中的大单词(WORD)

在Level 2中，我们学习了什么是Vim中的单词（word），现在我们看看Vim是怎么定义一个大单词（WORD）的。

* 由非空字符（空格、换行、tab属于空字符）构成的连续字串（例如：`vim-adventures`）

* `vim-adventures`是一个WORD，由3个word组成（`vim` `-` `adventures`）

##B与x

* B 光标向前移动到大单词（WORD）首字符
* x 删除当前光标所在字符

第三关有个要求30秒内取到钥匙的任务，用`w`和`e`就行，但手速一定要快。之后，用学到的`x`技能，删除地图上红色的字符，拿上小棕黄色钥匙返回第一关中的下图位置。

![]({{ site.imgs_url }}vim-adventures-level3-1.png)

站在上图中的感叹号位置，按`B`进入区域内。打开箱子，拿着蜡烛，然后你需要默默凭借直觉与记忆返回第三关的地图（如下图），与图中的紫色小人对话后，恭喜你，进入了第四关。

![]({{ site.imgs_url }}vim-adventures-level3-2.png)
