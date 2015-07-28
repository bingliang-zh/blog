---
layout: post
title: 在U盘上安装Ubuntu（Surface Pro3）
excerpt: "使用此方法不会影响Surface Pro3的Windows环境，不占用任何SSD空间，不修改SSD启动项。"
modified: 2015-07-28
tags: [ubuntu, surface pro 3, virtualbox, USB-stick]
comments: true
image:
  feature: sample-image-5.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
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

## 简介

我有一台Surface Pro 3 128gb版本，因为需要同时使用Windows环境和Ubuntu环境，所以我需要在默认基础上安装Ubuntu。但是因为SSD容量太小，而且并不想让修改Surface Pro3的启动项等等，所以最后决定制作一个独立的U盘Ubuntu系统。但是谷歌搜索U盘Ubuntu系统得到的大多是USB live这种的，而这种系统不会在U盘上保存任何信息，而我需要的是一个正常的Ubuntu系统，可以保存我安装的程序，文件等等。

在整个过程中经历了很多困难，包括用U盘重刷了整个Surface，安装Ubuntu驱动等等。所以在这篇博客的第一篇上我准备记录一下自己的步骤，给有同样需求的人以借鉴。

# 安装Ubuntu系统

## 准备

一个8G以上的U盘。建议16G以上，USB3.0（如果要进行编译等工作肯定需要一个大的磁盘空间，而且要获得良好的系统体验的话磁盘速度也很重要），我先是使用SONY晶雅16G安装成功，这次教程使用Sandisk CZ80 64G进行示范。

> [**Ubuntu 14.04.2 LTS 64位安装包(.iso)**](http://www.ubuntu.com/download/desktop)
> 
> 必须是64位不然不能用EFI启动。本文后半部分安装驱动的情况是以LTS版本为例，如果安装其他版本的Ubuntu可能需要自行编译Linux内核。

> [**VirtualBox 5.0.0和扩展包**](https://www.virtualbox.org/wiki/Downloads)
> 
> 分别下载VirtualBox 5.0 for Windows hosts和VirtualBox 5.0 Oracle VM VirtualBox Extension Pack。
> 
> 增加扩展包后可以支持USB3.0，应该可以使安装过程更快（未测试）。

下载下来的文件应该为：

| 文件名（官方版本更新的话文件名可能有变化） |
|:--------|
| ubuntu-14.04.2-desktop-amd64.iso  |
| VirtualBox-5.0.0-101573-Win.exe  |
| Oracle_VM_VirtualBox_Extension_Pack-5.0.0-101573.vbox-extpack |
{: rules="groups"}

## 安装VirtualBox和扩展包

先安装VirtualBox再安装扩展包，一路下一步，安装完即可。

## 配置虚拟机

新建一个虚拟机，名称随便填，版本选择Ubuntu (64-bit)，Surface Pro3是支持虚拟化和64位的，如果你在这里只能看到(32-bit)，而没有(64-bit)的选项，则搜索一下解决方案，这里不具体列出。

勾选了Ubuntu (64-bit)后点击下一步，内存大小默认就行，虚拟硬盘选择不添加虚拟硬盘（稍后解释）。然后点击创建就可以了。

选中新建的虚拟机，点击设置：

系统->主板->扩展特性，勾选**“启用EFI(只针对某些操作系统)”**；

存储->控制器，在光驱处载入下载的ubuntu安装包镜像。

USB设备，勾选**“启用USB控制器”**，点选**“USB 3.0 (xHCI)控制器”**，在右边筛选增加U盘，使用Sandisk CZ80时这里显示的是**"SanDisk Extreme [0010]"**，你自己的U盘可能有不同的设备名，请注意识别，可以比较一下插上和拔下U盘的USB设备列表。建议在进行下一步之前备份U盘的数据，也可以进行简单的格式化。

然后点击确定。

## 使用虚拟机安装Ubuntu

启动配置好的虚拟机。

稍等过后就会出现GRUB菜单。

选择Install Ubuntu并回车。

等待Ubuntu的载入画面结束后就进入安装向导了。

语言我这里选择了中文，然后断开网络连接（这样安装速度会更快），点击继续。

在安装类型这个界面可以选择默认选项或者选择其他选项。默认选项则会由Ubuntu自动给你分区，其他选项可以自己进行分区，然而因为可能是Win下对一个U盘只识别第一个分区，第一个分区一般是boot，所以分一个大一点的Fat32在Win下并不能使用。我这里选择默认选项。

选择好地区，语言和用户名等信息后就开始安装即可。在计算机名那里我把-Virtualbox改成了-Ubuntu（可选）。

等待系统安装完毕，CZ80只要不到5分钟。

然后点击重启键后关闭虚拟机即可。

## 修改EFI Boot

用7zip或别的解压缩软件打开ubuntu-14.04.2-desktop-amd64.iso，我现在的U盘的盘符为E:/，将iso中的EFI文件夹下的boot文件夹复制到E:/EFI中，复制E:/ubuntu/grubx64.efi到E:/EFI/BOOT下替换该目录下的grubx64.efi。

至此，一个可以从U盘启动的Ubuntu系统就制作完毕了。

## 进入Ubuntu
待续

# 更新Ubuntu内核（安装驱动）
这步的话还需要一个USB hub，