---
layout: post
title: "VIM Adventures Level 6"
description: ""
category: "Vim"
tags: [VIM Adventures]
---
{% include JB/setup %}


## 行首行尾移动与大小写转换

* `~`: 把当前字符进行大小写变换

* `$`: 移动光标到行尾

* `0`: 移动光标到行首

* `^`: 移动光标到行首第一个非空字符

## 第六关

第一个地图中限制在8步，我们需要逆向思维一下，先一直按j下来，然后底部向上进入地图，就可以8步删除红色区域了，得到一把钥匙。

然后往下走，消除右侧区域的红色字符，学会`~`。向下走，如图：

![]({{ site.imgs_url }}vim-adventures-level6-1.png)

由于步数限制，我们需要使用`~`来把b变为B，而不是`rB`，`X`来删除光标前一个字符，而不是使用`hx`，消除该地图后，学会`$`。向上走到大S处，用刚刚学会的`$`来过河到达!号。

右侧区域我们按照从上到下依次为：

![]({{ site.imgs_url }}vim-adventures-level6-2.png)

![]({{ site.imgs_url }}vim-adventures-level6-3.png)

![]({{ site.imgs_url }}vim-adventures-level6-4.png)