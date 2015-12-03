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
Update at 2015/11/20 11:23:42 

37wan Android SDK (37sdk_release_v2.1.2_20150910):

    public interface SQResultListener {
    	public void onSuccess(Bundle bundle);
    	public void onFailture(int code,String msg);
    }

这个渠道所有的回调都是调用了这个SQResultListener，所以我不得不在所有用到这个回调的地方看到这个不忍直视的错误。

----------
Update at 2015/11/27 15:40:01 

安趣SDK开发文档：

	12 混肴说明

	集成jar包后，如要混淆java代码，请不要混淆联编SDK jar包中的类。可以添加以下 类到proguard配置，排除在混淆之外：
	
	代码示例
	...

我的眼睛已经开始拆字了。

【难能可贵的是这个渠道是我碰到的第一个大陆用markdown写文档的，还做成了html】

----------
Update at 2015/11/30 17:16:43 

要玩SDK初始化：

    YWPlatform.getInstance().ywInital(info, new OnInitCompleteListener() {...});