---
layout: post
title: 在大陆开启Google Now
excerpt: "Shadowsocks使用小技巧"
modified: 2016/01/19 17:23:45  
tags: [Android, Google Now, Shadowsocks]
comments: true
image:
  feature: pixabay/library.jpg
  credit: Pixabay
  creditlink: https://pixabay.com/zh/%E5%BA%93-%E4%B9%A6%E7%B1%8D-%E4%B9%A6%E6%9E%B6-%E6%95%99%E8%82%B2-%E6%96%87%E5%AD%A6-%E5%AD%A6%E6%A0%A1-%E7%9F%A5%E8%AF%86-%E5%A4%A7%E5%AD%A6-%E6%99%BA%E6%85%A7-%E6%9E%B6-438389/
---

主要过程参考这一篇[**[GUIDE] [VIDEO] Enable Google Now / Now on Tap cards in any country [NO ROOT]**](http://forum.xda-developers.com/android/general/guide-enable-google-cards-country-t3102472)，经过我的测试，别的什么清Play Store都没什么用。

平台是Nexus 6，Android 6.0.1，MMB29S。使用工程包刷的机。

但是在wifi验证这里遇到了困难，视频中可以直接skip，我在公司的时候也可以skip，但是晚上在家里的时候skip按钮却变成了无法点击，这让我非常恼火。与此同时，我另外一只android上的fqRouter也无法正常工作，以前我都是用它来进行分wifi热点的工作的。

折腾了很久，发现自己非常蠢。其实很简单。

我Surface Pro 3的win8里面使用的是clowwindy大大的Shadowsocks-win。在PC上右击任务栏纸飞机图标，勾选“允许来自局域网的连接”，然后ipconfig查到本机的ip地址。然后在Nexus 6上面连接到同一个wifi，点击高级选项，代理点击手动，然后填上相对应的数值即可。

然后简单登录Google账号，但是不要选择同步（暂时不知道同步了还有没有用，你可以试试），然后参考上面那个连接的操作就可以了。

<figure>
	<a href="{{ site.url }}/images/2016-01-19-enable-google-now-in-mainland-china/1_google_now_screen_shot.png"><img src="{{ site.url }}/images/2016-01-19-enable-google-now-in-mainland-china/1_google_now_screen_shot.png"></a>
</figure>
