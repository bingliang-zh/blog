---
layout: post
title: 不忍直视的拼写错误
excerpt: "工作生活中碰到的许许多多拼写错误，吐槽搞笑向"
modified: 2015/11/20 11:22:46  
tags: [misspell]
comments: true
image:
  feature: nasa/airbos.jpg
  credit: NASA
  creditlink: http://www.nasa.gov/image-feature/stark-beauty-of-supersonic-shock-waves
---

一切的一切告诉我们，写代码的时候使用一个有拼写检查的IDE是多么的重要，最好像是Android Studio还可以自动识别驼峰等拼写方法的呢。

----------
Update 2015/11/20 11:23:42 

37wan Android SDK (37sdk_release_v2.1.2_20150910):

    public interface SQResultListener {
    	public void onSuccess(Bundle bundle);
    	public void onFailture(int code,String msg);
    }

这个渠道所有的回调都是调用了这个SQResultListener，所以我不得不在所有用到这个回调的地方看到这个不忍直视的错误。