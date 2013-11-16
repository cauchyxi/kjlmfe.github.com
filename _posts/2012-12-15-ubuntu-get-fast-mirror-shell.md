---
layout: post
title: Ubuntu自动更新最快软件源脚本
category: Linux
---

![get_fast_mirror]({{ site.imgs_url }}get_fast_mirror.png)

每次装好Ubuntu，对于大多数用户来说，首先要做的事就是手动修改/etc/apt/sources.list文件，将里面的官方软件源地址更换为自己学校或者公司的软件源。当我们更换一个工作环境后，可能伴随着又要替换旧的软件源地址。

笔者觉得这样每次手动更改软件源是一件及其麻烦重复的劳动，于是编写了一个自动更新最快软件源的脚本，从此一劳永逸。


## 原理

最直观的想法就是：对各个软件源进行测速，选出最快的那个，之后将其替换为新的软件源。

那么如何对各个软件源测速呢？有两种方法：

  一、用ping命令 测量其平均响应时间 选出响应时间最短的那个
  二、用wget命令 测量下载一个文件的总时间 选出耗时最少的那个

那么这两种方法有什么区别呢？我们该用哪个呢？

前者选出的是响应时间最优的，后者选出的是下载速度最快的。我们都知道软件源的作用是供客户端下载更新软件，所以当然是后者的方法更为准确，但笔者最终选择了前者作为测速方案，因为前者的用户体验更好且代码简单易懂。设想，如果我们采用后者，那么需要从每个软件源下载一个文件，并且这个文件不能太小，否则无法区分他们的速度，那么一个显而易见的情况是脚本需要运行较长的时间。

虽然存在某些软件源可能响应时间很短，而下载速度却很慢的情况，但经过笔者的多次实验，发现这样的情况并不常见。


## 实现

* 首先测试用户网络状态

{% highlight bash %} local speed=`ping -W1 -c1 www.baidu.com 2> /dev/null | grep "^rtt" |  cut -d '/' -f5`{% endhighlight %}
取出其平均响应时间，如果speed == "" 则说明网络不通，提示用户，且退出程序。否则，说明网络正常，继续执行。

* 检测软件源列表文件是否存在

{% highlight bash %} test -f $SOURCES_MIRRORS_FILE{% endhighlight %} 
若不存在，提示用户，且退出程序。

* 对每个软件源地址进行测速

在测速之前清空上次运行的测速结果文件，之后将每个软件源的测速结果(源地址 平均响应时间)写入测速结果文件

* 对测速结果进行排序

{% highlight bash %} sort -k 2 -n -o $MIRRORS_SPEED_FILE $MIRRORS_SPEED_FILE{% endhighlight %}
对每行记录 按照平均响应时间升序排列

* 选出最快的软件源

{% highlight bash %} head -n 1 $MIRRORS_SPEED_FILE | cut -d ' ' -f1{% endhighlight %}
选出最快的软件源

* 询问用户是否要使用该软件源

用户确认后，先对用户之前的软件源进行备份，然后再替换。


## 脚本源代码

[最新版本：https://github.com/KJlmfe/soEasyUbuntu/tree/master/get_fast_mirror](https://github.com/KJlmfe/soEasyUbuntu/tree/master/get_fast_mirror)


{% highlight bash %}
#!/bin/bash

#Program:
#	This program gets the fastest ubuntu software sources from SOURCES_MIRRORS_FILE
#	and backup && update /etc/apt/sources.list

#Author:  KJlmfe	www.freepanda.me

#History:
#	2012/12/6	KJlmfe	First release


VERSION="precise"  # precise is code of Ubuntu 12.04 if your ubuntu is not 12.04 please change
TEST_NETCONNECT_HOST="www.baidu.com"
SOURCES_MIRRORS_FILE="sources_mirrors.list"	
MIRRORS_SPEED_FILE="mirrors_speed.list"

function get_ping_speed()	#return average ping $1 time
{
	local speed=`ping -W1 -c1 $1 2> /dev/null | grep "^rtt" |  cut -d '/' -f5`
	echo $speed
}

function test_mirror_speed()	#
{
	rm $MIRRORS_SPEED_FILE 2> /dev/null; touch $MIRRORS_SPEED_FILE
	
	 cat $SOURCES_MIRRORS_FILE | while read mirror
	do
		if [ "$mirror" != "" ]; then
			echo -e "Ping $mirror \c"
			local mirror_host=`echo $mirror | cut -d '/' -f3`	#change mirror_url to host
	
			local speed=$(get_ping_speed $mirror_host)
	
			if [ "$speed" != "" ]; then
				echo "Time is $speed"
				echo "$mirror $speed" >> $MIRRORS_SPEED_FILE
			else
				echo "Connected failed."
			fi
		fi
	done
}

function get_fast_mirror()
{
	 sort -k 2 -n -o $MIRRORS_SPEED_FILE $MIRRORS_SPEED_FILE
	 local fast_mirror=`head -n 1 $MIRRORS_SPEED_FILE | cut -d ' ' -f1`
	 echo $fast_mirror
}

function backup_sources()
{
    echo -e "Backup your sources.list.\n"
    sudo mv /etc/apt/sources.list /etc/apt/sources.list.`date +%F-%R:%S`
}

function update_sources()
{
    local COMP="main restricted universe multiverse"
    local mirror="$1"
    local tmp=$(mktemp) 

	echo "deb $mirror $VERSION $COMP" >> $tmp
	echo "deb $mirror $VERSION-updates $COMP" >> $tmp
	echo "deb $mirror $VERSION-backports $COMP" >> $tmp 
	echo "deb $mirror $VERSION-security $COMP" >> $tmp
	echo "deb $mirror $VERSION-proposed $COMP" >> $tmp

	echo "deb-src $mirror $VERSION $COMP" >> $tmp 
	echo "deb-src $mirror $VERSION-updates $COMP" >> $tmp 
	echo "deb-src $mirror $VERSION-backports $COMP" >> $tmp 
	echo "deb-src $mirror $VERSION-security $COMP" >> $tmp 
	echo "deb-src $mirror $VERSION-proposed $COMP" >> $tmp

    sudo mv "$tmp" /etc/apt/sources.list
    echo -e "Your sources has been updated, and maybe you want to run \"sudo apt-get update\" now.\n";
}

echo -e "\nTesting the network connection.\nPlease wait...   \c"

if [ "$(get_ping_speed $TEST_NETCONNECT_HOST)" == "" ]; then
	echo -e "Network is bad.\nPlease check your network."; exit 1
else
	echo -e "Network is good.\n"
	test -f $SOURCES_MIRRORS_FILE

	if [ "$?" != "0" ]; then  
		echo -e "$SOURCES_MIRRORS_FILE is not exist.\n"; exit 2
	else
		test_mirror_speed
		fast_mirror=$(get_fast_mirror)

		if [ "$fast_mirror" == "" ]; then
			echo -e "Can't find the fastest software sources. Please check your $SOURCES_MIRRORS_FILE\n"
			exit 0
		fi

		echo -e "\n$fast_mirror is the fastest software sources. Do you want to use it? [y/n] \c"	
		read choice

		if [ "$choice" != "y" ]; then
			exit 0
		fi

		backup_sources
		update_sources $fast_mirror
	fi
fi
	
exit 0 
{% endhighlight %}

