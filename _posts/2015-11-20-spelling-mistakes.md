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

<section id="table-of-contents" class="toc">
  <header>
    <h3>目录</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

----------

## failure --> failture

**37wan Android SDK (37sdk_release_v2.1.2_20150910)**

    public interface SQResultListener {
    	public void onSuccess(Bundle bundle);
    	public void onFailture(int code,String msg);
    }

这个渠道所有的回调都是调用了这个SQResultListener，所以我不得不在所有用到这个回调的地方看到这个不忍直视的错误。

其实直接onFail也是可以的，fail也有名词的形式。

----------

## 淆 --> 肴

**安趣SDK开发文档**

	12 混肴说明

	集成jar包后，如要混淆java代码，请不要混淆联编SDK jar包中的类。可以添加以下 类到proguard配置，排除在混淆之外：

	代码示例
	...

我的眼睛已经开始拆字了。

【难能可贵的是这个渠道是我碰到的第一个大陆用markdown写文档的，还做成了html】

----------

## initial --> inital

**要玩SDK初始化**

    YWPlatform.getInstance().ywInital(info, new OnInitCompleteListener() {...});

其实直接init就可以

----------

## destroy --> destory

**拇指玩SDK3.1.1实例销毁**

    MzwSdkController.getInstance().destory();

居然这渠道的文档里面的destroy是拼对的。也许他们的文档书写人员用了word的拼写检查功能吧（但代码里还是错啊。）

----------

## login --> loign

**拇指玩SDK3.1.1登录**

    new MzwLoignCallback(){}；

天呐，写这个SDK的人打字真“快”。
