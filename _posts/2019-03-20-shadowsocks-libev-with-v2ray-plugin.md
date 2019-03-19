---
layout: post
title: 配置 shadowsocks-libev 加 v2ray-plugin
excerpt: "上网如逆水行舟"
modified: 2019/03/20 02:03:00
tags: [fq, shadowsocks, v2ray]
comments: true
image:
  feature: 2019-03-20-shadowsocks-libev-with-v2ray-plugin/mob-and-shishou.jpg
  credit: Pixiv - 冬木
  creditlink: https://www.pixiv.net/member_illust.php?mode=medium&illust_id=73600006
---

## TL;DR

起因是公司网内无法用ss了，可以参见这个 [issue](https://github.com/shadowsocks/shadowsocks/issues/1399)。

基本就是这三个 repo 的 readme 就可以解释清楚了。

- [https://github.com/shadowsocks/shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)
- [https://github.com/shadowsocks/v2ray-plugin](https://github.com/shadowsocks/v2ray-plugin)
- [https://github.com/Neilpang/acme.sh](https://github.com/Neilpang/acme.sh)


## the rest

就元旦回来，电信找公司切换了一下网络，然后发现 SS，ssh 都上不去（感觉是一个算法一段时间后就给你断掉）。本来以为是公司网络问题，因为手机和家里都是可以上的，也都是电信，怀疑给公司充钱拿低配。于是还去找了公司网管，网管就是说你这个服务器问题。不想跟他讲就找了公司管代理的，他说要加混淆。这个思路就很对。于是找了一下比较成熟的适合个人的方式。网上教程大多是 obfs，不过 obfs 的 [repo](https://github.com/shadowsocks/simple-obfs) 里面说 "Deprecated. Followed by v2ray-plugin." 那我就用一下后面一个吧，也没找到什么傻瓜教程，还好这些开源的文档写的都很清楚。

使用 https 或者 quic 混淆的话，需要一个证书。可以去 acme.sh 的 readme 中看。我选了自己正在使用的服务商提供的 api 进行了生成。

客户端可以直接原有的 shadowsocks-win 客户端，放一个 v2ray-plugin.exe 文件到 Shadowsocks.exe 同目录下即可。Android 可以下到 v2ray plugin，是给ss客户端的一个插件，用起来也很直白。v2ray-plugin 已经有编译好的文件直接在 v2ray-plugin 的 [releases](https://github.com/shadowsocks/v2ray-plugin/releases) 页面下下来即可，注意tar.gz解压后的二进制文件要改成正确的名称。
