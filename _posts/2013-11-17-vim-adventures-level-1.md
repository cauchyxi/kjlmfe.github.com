---
layout: post
title: "VIM Adventures Level 1"
description: ""
category: "Vim"
tags: [VIM Adventures]
---
{% include JB/setup %}

## Vim Adventures 介绍

[Vim Adventures](http://vim-adventures.com)是一个学习Vim的游戏。目前一共有12关，其难度由易到难，循序渐进的教会你使用Vim，前3关可以免费试玩，之后需要收费。我已经全部通关了，多谢送我账号的[孙志岗](http://blog.sunner.cn)老师。由于该游戏存在一定难度，尤其对于Vim新手来说，很容易卡在一些地方过不去，所以我准备写个Vim Adventures系列通关文章。

BTW：如果你非常喜欢这个游戏，但经济能力有限，我愿把账号共享给你。

## Leve 1 攻略

* h: 光标向左移动
* j: 光标向下移动
* k: 光标向上移动
* l: 光标向下移动

进入游戏后，你有四个初始按键技能：hjkl（如下图），分别代表光标移动的四个方向。

![]({{ site.imgs_url }}vim-adventures-level1-1.png)

你需要使用hjkl来移动你的位置，进入右方迷宫后，你会遇见小人，他们会告诉你一些有用的tips，建议认真看，然后吃到一把黄色钥匙，在迷宫出口有个门，需要你用钥匙打开才能通过。

出了迷宫后，需要穿过斜坡，这里有个小技巧，我们需要先移动到第一行末尾（如下图），然后一直按j（向下）就能穿过斜坡了，后面同理穿过多个斜坡后，第一关就结束了。

![]({{ site.imgs_url }}vim-adventures-level1-2.png)
