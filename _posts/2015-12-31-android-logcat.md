---
layout: post
title: Android Logcat 小技巧
excerpt: "年末日志"
modified: 2015/12/31 18:00:00  
tags: [Android,Logcat,Debug]
comments: true
image:
  feature: pixabay/library.jpg
  credit: Pixabay
  creditlink: https://pixabay.com/zh/%E5%BA%93-%E4%B9%A6%E7%B1%8D-%E4%B9%A6%E6%9E%B6-%E6%95%99%E8%82%B2-%E6%96%87%E5%AD%A6-%E5%AD%A6%E6%A0%A1-%E7%9F%A5%E8%AF%86-%E5%A4%A7%E5%AD%A6-%E6%99%BA%E6%85%A7-%E6%9E%B6-438389/
---

明天过年了，在过年前上传个github证明自己还活着吧。

平台为Windows 7，Ubuntu下应该更容易

看一个不是自己打的apk的话Logcat太多，于是比较头疼。

先使用[**ApkSpy**](http://forum.xda-developers.com/showthread.php?t=2710041)打开某个打包签名好了的apk文件。（新版本aapt不在android-sdk-windows\platform-tools中，在build-tools下最新版本文件夹中拷个aapt.exe到ApkSpy.exe同目录下）。得到Package，形如com.blStudio.myApp

然后把这个apk装到机器中，并从Google Play去下一个“终端模拟器”，运行apk，然后切换到终端模拟器中，打入

    ps | grep com.blStudio.myApp

也可以偷懒直接打

    ps | grep myApp

然后得到形如

    u0_a290 12637 396 1082284 153156 ffffffff 00000000 S com.blStudio.myApp

的字串。第二个数字就是process id简称PID。

打开Android Studio（或者Eclipse或者android-sdk-windows\tools\monitor.bat），新建一个filter，在PID栏中打入上一步中得到的数字，这个例子中打入12637。

然后就可以得到该程序的输出了。

【写本文是因为查了一圈直接通过packageName获得logcat的方法，但是不知道为什么没什么用，所以绕了一个大圈，还算解决】

【Ian Murdock去世了】
<figure>
	<a href="{{ site.url }}/images/2015-12-31-android-logcat/openlogo-50_blackribbon.png"><img src="{{ site.url }}/images/2015-12-31-android-logcat/openlogo-50_blackribbon.png"></a>
</figure>
