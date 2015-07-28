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

在安装类型这个界面可以选择默认选项或者选择其他选项。默认选项则会由Ubuntu自动给你分区，其他选项可以自己进行分区。64G全装Ubuntu不免显得有些浪费，所以这里我准备分出32G来当做普通的U盘使用。所以这里我选择其他选项，你也可以省事选择默认选项。

<figure>
	<a href="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/1_install.png"><img src="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/1_install.png"></a>
</figure>

在分区之前最好使用RMPrepUSB进行一下格式化，因为使用过的U盘容易出现各种问题，用Ubuntu分区就容易在分区之前或者之后多出那0MB或者1MB，虽然没有太大问题，但是分区表非常难看。

<figure>
	<a href="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/5_RMPrepUSB.png"><img src="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/5_RMPrepUSB.png"></a>
	<figcaption>勾上设置分区不可启动，选择FAT32文件系统</figcaption>
</figure>

下面是我的分区表的图：
<figure>
	<a href="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/2_partition.png"><img src="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/2_partition.png"></a>
</figure>

现在设备的最开始的地方建立一个32GB大小的fat32分区，然后挂载到/windows目录下。

> 为什么要在设备的最开始的地方建立32G的fat32分区？
> 
> 因为windows系统识别U盘的时候只能识别第一个能识别的分区，如果把EFI启动分区作为第一个分区，那么windows就只能把第一个分区挂载到系统里，而我想要挂载32G的分区用来当U盘。虽然只能识别一个分区，不过这也有一个好处，在USB病毒泛滥的打印店等我们就不用担心病毒会对EFI启动分区做什么手脚了。

然后在空余空间建立一个150MB的EFI启动分区，如果使用默认配置这个分区一般会有几百兆，但是我们仅仅装单系统，所以不用很大。

接着在空余空间的末尾建立2G的交换空间，再把剩下的所有空间都设为ext4分区，挂载到/目录下。

这样就完成了分区的工作，点击下一步。

选择好地区，语言和用户名等信息后就开始安装即可。在计算机名那里我把-Virtualbox改成了-Ubuntu（可选）。

等待系统安装完毕，CZ80只要不到5分钟，如果你的U盘比较慢或者没有安装VirtualBox扩展包的话那么可能要等待很久。我在用公司电脑装的时候还出现神秘报错，查错了一下午后来发现是前面板电流太小，就把前面板插着的手机拔掉就一切正常了。

然后点击重启键后关闭虚拟机即可。

至此Ubuntu就被安装到我的U盘之中了，不过要启动它还要一番功夫。

## 修改EFI Boot

用7zip或别的解压缩软件打开ubuntu-14.04.2-desktop-amd64.iso，或者直接挂载到虚拟光驱，把里面的EFI文件夹复制出来。

<figure>
	<a href="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/4_EFI.png"><img src="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/4_EFI.png"></a>
</figure>

> 如果之前分区的时候你是用默认配置，那么你会发现电脑挂载了一个几百M的磁盘，直接打开它，然后把从iso里面的EFI文件夹复制到该磁盘中，与原有的EFI文件夹合并，并且将EFI/ubuntu/grubx64.efi复制到EFI/BOOT目录下替换原有的同名文件即可。做完这些步骤后最好把整个EFI文件夹复制并重命名为EFI.backup，以便以后可能启动表损坏了之后进行复原。

因为用了自定义的分区，所以只能挂载32G的大分区，而150M的启动分区则不能在Windows的资源管理器中显示出来，但是可以在Ubuntu中全部显示。所以我们接下去再使用虚拟机来进行复制工作。先把.iso文件中的EFI文件夹复制到32G的分区里面

<figure>
	<a href="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/3_sandisk.png"><img src="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/3_sandisk.png"></a>
	<figcaption>只有一个32G的分区在资源管理器中显示</figcaption>
</figure>

再次打开虚拟机，记得再次载入.iso。这次选择Try Ubuntu without installing。把U盘载入Ubuntu系统，可以在Devices中看到四个设备，进入33G的那个设备，把EFI文件夹（就是刚才复制到U盘中的文件夹）**剪切**到150M的那个设备中，将两个EFI文件夹合并，然后同样将EFI/ubuntu/grubx64.efi复制到EFI/BOOT目录下替换原有的同名文件即可。

<figure>
	<a href="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/6_copyEFI.png"><img src="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/6_copyEFI.png"></a>
</figure>

关闭虚拟机。

至此，一个可以从U盘启动的Ubuntu系统就制作完毕了。

## 进入Ubuntu
Surface Pro 3可以参考:
> [**微软官网手册**](https://www.microsoft.com/surface/zh-cn/support/storage-files-and-folders/boot-surface-pro-from-usb-recovery-device#)

完全关闭电脑后，插上制作好的USB Ubuntu系统盘，按住音量减小键，按下并释放电源键，当显示Surface徽标时，释放音量键小键，就启动了U盘上的系统。

也可以按住把上面的流程中的音量减小键全部替换成音量增大键，进入类似bios的界面，Configure Alternate System Boot Order选项从**SSD Only**改为**USB -> SSD**然后保存并退出。然后就可以重启进入Ubuntu系统了。

正常情况下是左图，如果出现黑屏只有grub的画面则检查32G那个分区里的EFI文件夹是否还存在，如果存在的话则删掉它。

但是光这样的话还有很多使用上的问题，比如最重要的，type cover无法正常使用。如果要正常使用这些功能的话，则需要更新一下Ubuntu的内核。

# 更新Ubuntu内核（安装驱动）

感谢[**rubiojr**](https://github.com/rubiojr)的[**surface3-kernel**](https://github.com/rubiojr/surface3-kernel)项目。

更新内核需要键盘，但是type cover和蓝牙都不能用，所以需要一个USB hub，而USB键盘和鼠标应该都有吧。

> [内核下载地址](http://surface3.rbel.co/kernel/)

把这个页面的所有带有3.16.0的.deb包下载到本地。这些.deb包都是已经编译好了的，直接使用即可，如果Ubuntu版本不是14.0.2 LTS的话可能需要按照[**surface3-kernel**](https://github.com/rubiojr/surface3-kernel)的编译步骤来编译出需要的deb包。

把下载下来的4个.deb文件放到一个新建的kernel文件夹中，用各种方式（boot分区中转，空余容量中转，或者拿另一个U盘来中转）拷到用户主文件夹下。

除了内核还可以增加对触摸板的支持，并进行蓝牙/Wifi固件的安装。这两个都很简单，按照[**surface3-kernel**](https://github.com/rubiojr/surface3-kernel)下面的Touchpad support和Bluetooth/Wifi说明操作即可。不过无线还有不稳定的情况。

然后Ctrl+Shift+T打开终端，逐条输入命令

    安装内核：    
    sudo dpkg -i kernel/*.deb    
    sudo update-grub
    
    增加触摸板支持：    
    sudo cp -i xorg.conf /etc/X11/xorg.conf 
    
	蓝牙/Wifi固件安装：
    sudo cp mrvl/* /lib/firmware/mrvl/ 
    
	重启：
	sudo reboot

在启动项里面选择**Ubuntu 高级选项**->**Ubuntu, Linux 3.16.0-rc6-surface3**即可使用type cover了。（不过有个bug，type cover在开机状况下拿下来再装上去就无法使用了。）wifi连不上时先关机再开机一般可以解决，直接重启有时会有问题。

> 如何调整该内核为默认选项还在研究中。

<figure>
	<a href="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/7_final.png"><img src="{{ site.url }}/images/2015-07-28-how-to-build-an-usb-Ubuntu-on-surface-pro-3/7_final.png"></a>
</figure>

不管怎么说，最后我们就可以愉快地在Surface Pro 3上面使用Ubuntu了！