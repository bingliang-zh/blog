---
layout: post
title: 聊聊live2d
excerpt: "本来想写技术向的但是变成了瞎扯"
modified: 2015/08/31 16:35:15 
tags: [live2d]
comments: true
image:
  feature: pixiv/kyudo.jpg
  credit: Pixiv - 幻象黑兔
  creditlink: http://www.pixiv.net/member_illust.php?mode=medium&illust_id=45046654
---

<section id="table-of-contents" class="toc">
  <header>
    <h3>目录</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

## 前言【然而和正文并没有什么关系】

选了个弓道的P站的图来作为题图。高中的时候去过日本，寄宿家庭带我去了一个弓道馆，那种静与动、力量与柔美的矛盾与调和，太美。

<figure>
	<a href="{{ site.url }}/images/2015-08-28-sketch-about-live2d/1_real_kyodo_club.jpg"><img src="{{ site.url }}/images/2015-08-28-sketch-about-live2d/1_real_kyodo_club.jpg"></a>
	<figcaption>日本·福冈 2010-06-12</figcaption>
</figure>

弓道里有个术语叫做：正射必中。字面意思就是，射箭的话，姿势对了就能中靶。在我的感觉中这跟写代码一样，不能一股脑地冲着目标而去，而要把整个过程像分解动作一般分成一个一个模块，然后把每个模块都给写好，这样最后必然能够完成目标。尤其对初学者来说一定要踏实，走一步再走一步。在遇到各种错误而沮丧丢失信心时一定要相信“正射必中”这个理念。

## 正文

Live2d是一个日本软件商开发的一系列解决方案，用二维贴图和一系列还比较简单的控制块来做出非常精美的动态形象。

<video id="promotionVideo" controls preload="auto" class="video-js vjs-default-skin col-xs-12" poster="http://www.live2d.com/wp/wp-content/themes/Live2Dv2/images/importance.png">
    <source src="http://www.live2d.com/wp/wp-content/themes/Live2Dv2/movie/live2d_pv_en.webm">
	<source src="http://www.live2d.com/wp/wp-content/themes/Live2Dv2/movie/live2d_pv_en.ogv">
	<source src="http://www.live2d.com/wp/wp-content/themes/Live2Dv2/movie/live2d_pv_en.mp4">
<p class="vjs-no-js">To view this video please enable JavaScript, and consider upgrading to a web browser that <a href="http://videojs.com/html5-video-support/" target="_blank">supports HTML5 video</a></p>
</video>

Live2d几乎全平台制霸，所以不必担心使用的范围。日本人也拿它做过很多小邪恶的GAME等等，但是发挥你的想象你也可以做很多别的事情。我用Live2d做了一个语音助手，相比语音助手这种纯语音的，一个萌萌的形象更能让我接受。这个项目的内容可以查阅[github](https://github.com/thatblstudio/reimu)，对应[论文](https://github.com/thatblstudio/reimu/blob/master/%E5%9F%BA%E4%BA%8ELive2d%E6%8A%80%E6%9C%AF%E7%9A%84%E8%99%9A%E6%8B%9F%E5%BD%A2%E8%B1%A1%E7%89%A9%E8%81%94%E7%BD%91%E4%BA%A4%E4%BA%92%E8%BD%AF%E4%BB%B6-%E5%91%A8%E7%A7%89%E4%BA%AE%20%E6%AF%95%E8%AE%BE%E6%8A%A5%E5%91%8A.pdf)也在上面。你可以在googleplay搜索“灵梦”就可以下载发布版。

Live2d核心其实就是对二维贴图绑定图像锚点（跟三维建模时候的贴图有点像），通过更改锚点位置来对贴图进行各种变形（很简单的仿射变换等等），然后通过更“高层”的参数来控制这些锚点的关键帧的值，最后播放动画的时候是通过对参数进行赋值来对应渲染每帧的图像的，而关键帧之间的数值应该是线性填充的。这个过程从技术上来说完全没有任何的新鲜和创意，但是Live2d做出了一个相对高效而且也挺好用的软件，又有几乎全平台的SDK（照例没有WP），工具非常好用，所以为什么要重复发明轮子呢？况且该公司对于独立开发者还是比较友好的。

国内还没见人能够有做出原创角色，大部分的要么是Live2d官方发布的，要么就是从已有的日本游戏里面提取出来的模型，像我自己做的灵梦的模型这种我起码是没有搜到过。

B站上有一个人气很高的Live2d相关的up，做了一个基于歌词的口型同步。Up说做了一年，即将众筹，口气上是比较想卖钱，看来不是开源的。因为我的APP里面有说话的相关功能，所以当初想过根据文字来控制嘴型（其实就是两个参数），最后室友跟我说，你直接让模型自己动好了，我想想有道理，就写了一行正弦函数，然后毕设展总长三天来看的多少人也没觉得有差异，倒是给我提了很多很多别的建议啥的。日本动画也不像美国动画一样对嘴型K帧那么严格，所以不特意去看的话根本没什么违和感。而且我感觉实现那个Up所说的技术也并不那么难（需要一年什么的），写个脚本把日文弄成五十音，然后对五十音每个进行对应的K控制嘴型的那两个参数，然后写个脚本根据歌词把.lrc改成嘴型上去就行了呗，感觉并没有什么技术难点啊，也不需要一年啊，不工作的话一个程序一个星期应该连开发带测试也够了啊。然后看着Up要去众筹了，祝好咯。做一个播放器倒也是挺好玩的。也许有人要说，你行你上咯。嗯...假如需要音乐播放器功能的话。

也许我把我自己的演示视频做的“苹果”一点说不定点击量会高一点，但是感觉花这些时间还不如去写代码，写功能，比如把官方出的那个男性角色加进去。这个还是在毕设展上一个小姑娘玩了一会儿后跟我提的建议，她对虚拟的妹子没啥好感，倒是喜欢帅哥啥的。

结语：

其实我对于这些一切的想法来于一次一个人的动车，我寂寞地蛋疼地寂寞，然而打开手机看见的都是一堆一堆的panel, button, scroll bar...它们不停地跟我说，我的交互很棒，我的图标经过了半透明，有圆角，符合人的直觉。然而我跟它们说，你们都错了。

我想看到一个不那么机器的概念，跟我说说话，让我摸摸它。

我希望有个软件，她能够在乎我，至少看起来是这样。

人际关系不也如此么。