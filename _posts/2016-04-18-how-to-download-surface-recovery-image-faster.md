---
layout: post
title: 怎么下surface的恢复映像
excerpt: "巨硬药丸"
modified: 2016/04/19 23:56:01  
tags: [Microsoft,Surface,VNC,Digital Ocean]
comments: true
image:
  feature: 2016-04-18-how-to-download-surface-recovery-image-faster/win10-dsp-girl.jpg
  credit: 窓辺とおこ@windows10_toko
  creditlink: http://tweez.net/windows10_toko/
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

------

# 背景

我的128G专业版的Surface Pro 3的磁铁脚脱胶了，使用的时候真的是很怕它断啊，于是我就把它送回微软保修了，和售后说的很清楚是维修，但是两周过去，突然间他们就给我换了一个新的。配置升到了8G+256G。算是意外之喜，但是把我的笔贴（我贴在了Surface背上）和贴着的一堆很难直接买到的贴纸都给顺走了，包括从美国快递回来的Atom系列几张，WRTNode的贴纸，Genduino的贴纸，还有Vans鞋的“Off The Wall”贴纸。但是有一个大问题成为了有了这篇文章的原因。

这机子只能装Win10！我是一个Win8.1的Metro粉，在Win10刚出的时候我就升级了，但是对触摸的支持实在是太差了，而且后期的“软强制升级”实在是很失大厂风范，所以我后来都干脆装回Win8.1关闭系统更新了。话说回来，这台是Surface Pro 3生产线后期换成“田”牌之后的版本，预装的是Win10，而且去微软官网登陆下载recovery image的时候只给出了Win10的选项，而相对的我寄回返修的那个则是有两种不同的映像可供下载的。我二话不说从硬盘里翻出了之前从百度云下载的Win8.1映像开始重装，但是出现了诸如“无法激活”，“bitlocker出错”，“无法登陆”等一大堆问题，我甚至都开出了注册机开始在Surface上面激活Windows。我讨厌这样的商业软件公司。

但是我只是拿Surface当个便宜的新帝啊，于是最后我还是妥协准备装回Win10。可能是这个比较新，所以并搜不到什么第三方的下载，有一个分成两个包的百度网盘分享，但是下下来之后用7zip解压说缺了一个part。而用另一个找到的单zip包感觉有点奇怪，自动命名了电脑名，我修改后并激活bitlocker之后在微软官网显示的名称是原来的电脑名。因为想到了源码攻击这回事，以及对百度云的某种程度上的不信任，以及对这个文件到底该怎么下产生了浓重的好奇，我准备来干它一发。

# 尝试

## 使用不同浏览器并尝试配置代理

1. 在Win8.1平台上直连先后使用IE和Chrome，下载都是10K/s左右，明显不能用。
2. 以上条件不变，挂代理，一个shadowsocks.com的日本linode节点可以达到230K/s左右，看了下大概6-8个小时内可以下完，准备看看运气，因为shadowsocks.com的网络很不稳定，平常用来看看Youtube和别的，感觉这种官网下面挂着“使用本站服务请遵守中国大陆以及香港法律”的都是有点问题的。但是自己的DigitalOcean的多伦多节点还是太慢，用浏览器下载还是太容易挂。不出所料半个小时后shadowsocks.com的linode节点断了，这个时候大概下载了60M左右，这种方式还是不行。

## 使用不同系统

打开了Ubuntu 15.04，使用chromium直连，居然达到了300+K/s的速度。这让我说什么好。可是下载到了600M左右的时候还是断了，对于一个7G大小的zip包，这种方法还是不行。

## 尝试使用GUI下载工具

我在win下使用QQ旋风，Ubuntu下使用Uget。但是微软下载地址每一次都会生成一个不同的Key。

    https://dl-proc.ds.microsoft.com/DSDownloadAPI/Download.ashx?cmd=1&DownloadFileID=9591505&DownloadKey=a2877a11-9ad3-4a44-b266-5a98ddcf39fe&SiteID=1&Mode=0

尝试多次会发现只有DownloadKey这个键值会每次自动更新，但是这个我没法破解生成方式，所以放弃。复制url至旋风不会有任何下载进度，在Uget中直接报400。

无法使用GUI下载工具。

## 尝试使用命令行

1. 使用wget。我使用了包括伪装代理名称在内的方式下载，依然报400，大概还是因为那个DownloadKey卡住了我的去路。wget命令可以从这里查到：[http://man.linuxde.net/wget](http://man.linuxde.net/wget)
2. 使用命令行的浏览器。我的想法是使用putty连上多伦多的服务器，然后一个命令行的浏览器来生成对应的url。我尝试了links等软件，但是好像并不是那么好用，我没找到对应的下载按钮。

## 尝试VNC

**That finally works!**

这里有一个非常好的关于如何在DigitalOcean上安装VNC并连接的教程：

[How to Install and Configure VNC on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-14-04)

但是在"Step Four — Connect to Your VNC Desktop"中对于putty的使用有点不清楚。一步一步的说就是除了在Session中填上对应的ip后需要在Connection-->SSH-->Tunnels中填一些东西：Source port: `5901`, Destination: `localhost:5901`。

一切正常的话输入对应的VNC密码就可以打开一个非常卡的VNC界面。使用256色模式可以稍稍加速。然后经过了5分钟的无比迟钝的GUI操作之后，我终于在VNC中的Firefox中打开并登录了对应网站获得了下载按钮！然后就开始下载了，而这时的速度是14.1MB/s！6-7分钟后，一个7G大小的zip就下载到Download文件夹中。

## 把服务器中的文件下载下来

我先尝试了一下winscp。这个下载速度，放弃吧。

还不如自己搭一个lemp用来下载。这个也非常简单，依照这个教程即可：

[**How To Install Linux, nginx, MySQL, PHP (LEMP) stack on Ubuntu 14.04**](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-14-04)

然后就是简单的mkdir和mv操作把recovery image的zip放到nginx的html目录下。

在本地使用QQ旋风获得了大概800K/s的不是很稳定的速度。

~~如果你要下的文件名刚好是`SurfacePro3_BMR_40_7.102.0.zip`的话，我服务器中还暂时存放着该文件，但是因为我的droplet容量较小，所以如果我空间不够用的话我有可能把它删掉，毕竟一个7G大小的包呢！我也可能考虑使用种子的方式来进行分享。~~

~~下载地址：~~

~~**http://blstudio.cloud/download/SurfacePro3_BMR_40_7.102.0.zip**~~
（现已被移除）

MD5: 1848d2ac06f38d0b33b96de6a5ce95d0
SHA1:1a1204cdff2d917696d9c24f1112908b7d3b9f81

经过比对MD5和SHA1，该文件和我从百度网盘上下的是一样的：[**http://tieba.baidu.com/p/4456759293**](http://tieba.baidu.com/p/4456759293)

所以说，微软的这个官方映像在初始化设置的时候就漏了一步让用户选择电脑名字的步骤。不过这个可以在安装完成之后可以在【所有设置-->系统-->关于】里面修改

如果你要下的文件名不是我这个的话，可以参照我的方式也创建一个DigitalOcean的droplet，然后下载自己的对应recovery image。

DigitalOcean使用按时计费的方式，每个月5美刀，分到你实际的下载时间（视你的带宽大概为数个小时到数天不等）你可能只需要花很少的钱（想想淘宝15一个下载链接吧），下载到来源绝对正规的微软recovery image。

当然所有你拥有的VPS或者其他凡是能够得到root权限的服务器都是可以进行上述操作。

# 下面是安利

使用DigitalOcean的话，注册账号，激活邮箱等，然后就可以获得$10奖励，绑定信用卡或者充入$5后就可以正式开始使用，如果只是下一个对应文件的话，你可以使用一张信用卡，下完就把服务器删除，附赠的奖励完全够你下载（可以用两个月呢），如果担心信用卡信息的话也可以使用paypal充值的方式。

（附上我的邀请链接：[**https://m.do.co/c/02e0e3fb51f8**](https://m.do.co/c/02e0e3fb51f8)，通过这个链接注册的话除了你可以获得和自己注册一样的奖励外，我还可以获得另外一笔$10推广奖励~）
