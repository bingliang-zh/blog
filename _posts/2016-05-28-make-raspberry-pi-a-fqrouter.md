---
layout: post
title: 做个树莓派翻墙路由器
excerpt: "网上有很多教程，但这个应该是最新的>_>"
modified: 2016/05/30 04:05:35
tags: [RaspberryPi, shadowsocks, GFW, openwrt, router]
comments: true
image:
  feature: 2016-05-28-make-raspberry-pi-a-fqrouter/raspberry_pi_wallpaper_pcb_by_rbininger-d5wf4xx.png
  credit: DeviantArt - rbininger
  creditlink: http://rbininger.deviantart.com/art/Raspberry-pi-wallpaper-PCB-356784837
---

<section id="table-of-contents" class="toc">
  <header>
    <h3>做个树莓派翻墙路由器</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

------

## 前言

照例的絮絮叨叨。起因是给chromebook装crouton的时候各种网络问题（不过后来熟练掌握的crouton的proxy用法后这些问题已经不复存在），所以想要装一个openwrt的翻墙路由器。起初淘宝买了一个洋垃圾Netgear WNDR3800，后来没用上就退掉了。看着毕设时候买的Raspberry Pi 2，一直闲置着，翻了一下openwrt支持列表，算是对Pi2支持很不错（我当初也想在手头的WRTNode上面刷一个Shadowsocks但是也是各种出错，有需求的时候再试试看）。于是开始自己倒腾起了。网上的资料很多但是相对凌乱，而且它们的设备较老，所以本日志是对它们的一个更新补充。

## 我用到的设备

- Raspberry Pi 2 B
- EDUP EP-N8508GS
- TF卡
- 支持HDMI的显示器
- 键盘
- Linux Laptop
- 主路由器

## 网络拓扑图

![Alt text](http://g.gravizo.com/g?
  digraph G {
    aize ="4,4";
    Internet;
    Internet -> mainRouter [label="cable"];
    mainRouter [label="Main Router\n192.168.1.1" shape=box];
    mainRouter -> laptop [style=dotted, label="wifi"];
    laptop [label="Linux Laptop\n192.168.1.x"];
    mainRouter -> raspi [label="cable\n192.168.1.4", weight=0];
    raspi [label="Raspberry Pi\n192.168.2.1" shape=box];
    raspi -> someDevice [label="wifi"]
    someDevice [label="some device"]
  }
)

## 编译openwrt及shadowsocks相关ipk

至本日志发布为止，Raspberry Pi 2 B支持的最新版本为15.05.1 Chaos Calmer。所以编译目标为该版本。

编译前确保电脑内存和硬盘足够，当初我在Digital Ocean上的最低配置的VPS或者在我的Chromebook上编译会有问题，最后我使用了PC+Virtualbox+Ubuntu的方式，给虚拟机分配了2核+8G内存+40G动态大小硬盘。编译时需要链接网络，所以需要保证联网，有些源会被墙，会自动寻找可用源，也有可能失败，失败了重新编译就行，不会清除已经编译好的文件，编译时使用proxychains-ng会出错，原因未名。

参考：[OpenWrt build system – Installation](https://wiki.openwrt.org/doc/howto/buildroot.exigence)

安装必须的程序

    sudo apt-get update
    sudo apt-get install git-core build-essential libssl-dev libncurses5-dev unzip gawk

安装可能需要的程序

    sudo apt-get install subversion mercurial

下载源码（这里下载的是15.05版本的源码，直接下载很慢，可以使用代理）

    git clone git://git.openwrt.org/15.05/openwrt.git

下载并安装可用的"feeds"

    cd openwrt
    ./scripts/feeds update -a
    ./scripts/feeds install -a

下载[Shadowsocks-libev for OpenWrt](https://github.com/shadowsocks/openwrt-shadowsocks)和[OpenWrt-dist LuCI](https://github.com/aa65535/openwrt-dist-luci)源码（下载慢可用代理）

    git clone https://github.com/shadowsocks/openwrt-shadowsocks.git package/shadowsocks-libev
    git clone https://github.com/aa65535/openwrt-dist-luci.git package/openwrt-dist-luci

编译 po2lmo（OpenWrt-dist LuCI需要po2lmo）

    pushd package/openwrt-dist-luci/tools/po2lmo
    make && sudo make install
    popd

编译目录

    make menuconfig

编译完成后会弹出编译目录，在这里进行一些选择：

- Target System --> Broadcom BCM2708/BCM2709
- Subtarget --> BCM2709 based boards
- Target Profile --> Raspberry Pi 2
- LuCI --> 3.Applications --> luci-app-shadowsocks-spec（用M选择，这样编译出来的是独立的ipk包）
- LuCI --> Network --> shadowsocks-libev-spec-polarssl（同样用M选择，也可以顺便把所有前缀是shadowsocks的都选上）

Save & Exit

编译

    make V=s

等待几小时，完成后可以在openwrt/bin目录下看到新生成的brcm2708文件夹。可以在brcm2708/packages/base目录下找到shadowsocks-libev-spec-polarssl_2.4.6-1_brcm2708.ipk和luci-app-shadowsocks-spec_1.4.0-1_all.ipk。虽然同时在brcm2708目录下可以找到编译出来的openwrt-brcm2708-bcm2709-sdcard-vfat-ext4.img，但是这个镜像安装之后opkg install会报错，所以编译这一步只使用编译出来的两个ipk文件，openwrt映像还是使用从官网下载的版本。编译ipk也可以使用openwrt sdk，但我下载的sdk编译报错，所以使用直接编译全部的方式。

## 烧录openwrt镜像

把TF卡连上电脑进行烧录。

参考：[Raspberry Pi[OpenWrt Wiki]](https://wiki.openwrt.org/toh/raspberry_pi_foundation/raspberry_pi)

在OpenWrt的树莓派页面上有对应的下载链接和安装教程。

下载下来后执行：

    dd if=/home/username/Downloads/openwrt-brcm2708-bcm2709-sdcard-vfat-ext4.img of=/dev/sdX bs=2M conv=fsync

上面这行shell命令中的sdX会不一样，可以使用dmesg，也可以使用gparted来找到插入的tf卡的设备名称。

windows上可以使用Win32DiskImager。

烧录完之后就可以插入TF卡，链接上显示器键盘网卡和电源线进行初步的设置了。

## 对树莓派进行初步设置

插上电源后等待10+秒后，显示器没有更新信息后按回车进入控制台。

### 修改密码

    passwd

按照提示输入，这一步会自动开启SSH

### 修改network配置

参考：[DIY高性能树莓派OpenWrt无线路由器 - 树叶的blog](http://www.shuyz.com/install-openwrt-on-raspberry-as-a-wireless-router.html)

    vi /etc/config/network

删除lan口下ifname那行，修改lan下ipaddr地址为另一个网段（我的主路由网段是192.168.1.x，所以此处我修改为192.168.2.1），新增一个wan口，设为动态获取。

    config interface 'loopback'
            option ifname 'lo'
            option proto 'static'
            option ipaddr '127.0.0.1'
            option netmask '255.0.0.0'

    config interface 'lan'
            option type 'bridge'
            option proto 'static'
            option ipaddr '192.168.2.1'
            option netmask '255.255.255.0'
            option ip6assign '60'

    config interface 'wan'
            option ifname 'eth0'
            option proto 'dhcp'

    config globals 'globals'
            option ula_prefix 'xxxx:xxxx:xxxx::/xx'

用网线连接上Raspberry Pi的网口和主路由的LAN口，然后执行`/etc/init.d/network restart`重启网络，然后就可以使用`ping`命令检测网络连接。如果能够ping通的话就可以进行下一步了。

### 修改firewall设置

    vi /etc/config/firewall

参考：[如何配置防火墙 [OpenWrt Wiki]](https://wiki.openwrt.org/zh-cn/doc/uci/firewall)

在firewall中新建两条规则，分别开启从外网可以访问22和80端口，这样Raspberry Pi连上主路由之后就可以在主路由的局域网里通过SSH和LuCI访问Raspberry Pi了。

    config rule
            option src              'wan'
            option dest_port        '22'
            option target           'ACCEPT'
            option proto            'tcp'

    config rule
            option src              'wan'
            option dest_port        '80'
            option target           'ACCEPT'
            option proto            'tcp'

保存后重启网络和防火墙

    /etc/init.d/network restart
    /etc/init.d/firewall restart

## 通过SSH连接到Raspberry Pi

Raspberry Pi在主路由上获取到的IP是192.168.1.4，现在可以在连到主路由的设备上通过SSH或者LuCI来设置。SSH则是直接`ssh root@192.168.1.4`，LuCI则是在浏览器中直接访问`http://192.168.1.4`即可。

## 安装网卡驱动

更新源（这个时间有时候会很长，如果失败的话重新执行该命令）

    opkg update

安装USB设备支持

    opkg install kmod-usb-core kmod-usb-uhci kmod-usb-ohci kmod-usb2
    opkg install usbutils

查看网卡芯片

    lsusb

我的是：Realtek Semicondoctor Corp. RTL8188CUS 802.11n WLAN Adapter。这一款可以用rtl8192cu来作为驱动。

参考：[ RTL8188CUS - two wireless interfaces possible? (AP+STA) --General Discussion --OpenWrt](https://forum.openwrt.org/viewtopic.php?id=60967)

安装rtl8192cu驱动

    opkg install kmod-rtl8192cu

安装完成后重启

    reboot

重启之后查看是否成功安装了设备驱动

    root@OpenWrt:~# lsmod | grep 8192
    compat                  1476  4 rtl8192cu
    mac80211              388426  3 rtl8192cu
    rtl8192c_common        32617  1 rtl8192cu
    rtl8192cu              58304  0
    rtl_usb                 8087  1 rtl8192cu
    rtlwifi                48071  2 rtl8192cu

## 修改无线设置

默认的无线是关闭的，需要把它打开。

    vi /etc/config/wireless

删除disabled那一行，确认channel的值不是auto。此时的Wifi设置是没有加密的。

但是不知为何Wifi信号是搜不到的，打开LuCI页面可以看到Wifi状态显示的是："Wireless is disabled or not associated"，在这里我折腾了很久，后来发现下一步可以修正它，让无线正常工作。

## 增加加密模式的支持

参考：[Configure WiFi encryption [OpenWrt Wiki]](https://wiki.openwrt.org/doc/uci/wireless/encryption)

先更新源

    opkg update

安装支持（官方建议安装wpad-mini），从支持情况来看，可能是默认的模式不支持AP，但是未作研究。

    opkg install wpad-mini

接下去使用LuCI（或者直接编辑wireless文件）来修改模式为WPA2。然后重启。

    reboot

现在正常的话你可以在你的Wifi列表之中看到设置的Raspberry Pi上的无线AP信号了。

我的wireless文件：

    config wifi-device 'radio0'
        option type 'mac80211'
        option path 'platform/bcm2708_usb/usb1/1-1/1-1.5/1-1.5:1.0'
        option hwmode '11g'
        option channel '1'
        option txpower '20'
        option country '00'

    config wifi-iface
        option device 'radio0'
        option network 'lan'
        option mode 'ap'
        option ssid 'Raspi'
        option encryption 'psk2+ccmp'
        option key 'password'

## 安装Shadowsocks

先通过SSH连接上Raspberry Pi，然后使用SCP命令将先前编译好的文件复制到Raspberry Pi上。

复制文件到Raspberry Pi上的/tmp文件夹中。/tmp文件夹下的文件会在每次重启后自动清空，所以直到安装前不要重启，安装完成后直接重启就会自动清除。

    scp raspi/shadowsocks-libev-spec-polarssl_2.4.6-1_brcm2708.ipk root@192.168.1.4:/tmp
    scp raspi/luci-app-shadowsocks-spec_1.4.0-1_all.ipk root@192.168.1.4:/tmp

    ssh root@192.168.1.4
    cd /tmp
    ls

输完ls后你可以看到复制过来的两个文件。

下面开始安装：

    opkg update
    opkg install shadowsocks-libev-spec-polarssl_2.4.6-1_brcm2708.ipk
    opkg install luci-app-shadowsocks-spec_1.4.0-1_all.ipk

安装完成后登陆LuCI，可以在Service下发现新增了shadowsocks的目录。不过现在登陆Shadowsocks账号还缺一个步骤，设定DNS。

## 设置DNS

参考：[路由器刷OpenWRT安装shadowsocks使用透明代理+去DNS污染 - 落格博客](https://www.logcg.com/archives/860.html)

Raspberry Pi使用opkg没法安装wget，所以这里还是使用scp来中转。

在Raspberry Pi上新建一个`dnsmasq.d`的目录，这里边存放dns规则。

    mkdir /etc/dnsmasq.d

在`/etc/dnsmasq.conf`中写入`conf-dir=/etc/dnsmasq.d`使dnsmasq.d目录内的规则生效

    echo "conf-dir=/etc/dnsmasq.d" >> /etc/dnsmasq.conf

下载下面两个文件并使用scp拷贝到Raspberry Pi上的`/etc/dnsmasq.d`文件夹中

[https://raw.githubusercontent.com/felixonmars/dnsmasq-china-list/master/accelerated-domains.china.conf](https://raw.githubusercontent.com/felixonmars/dnsmasq-china-list/master/accelerated-domains.china.conf)

[https://raw.githubusercontent.com/felixonmars/dnsmasq-china-list/master/bogus-nxdomain.china.conf](https://raw.githubusercontent.com/felixonmars/dnsmasq-china-list/master/bogus-nxdomain.china.conf)

    scp raspi/accelerated-domains.china.conf root@192.168.1.4:/etc/dnsmasq.d
    scp raspi/bogus-nxdomain.china.conf root@192.168.1.4:/etc/dnsmasq.d

使用SSH登陆Raspberry Pi

    cd /etc/dnsmasq.d
    ls

你可以看到刚才下载的两个文件。

新建一个配置文件，意义为让其他不在列表中（即国外域名）都走代理解析。这里的5300是设定的用来UDP转发的端口，Shadowsocks安装完成后默认的就是5300，也可以自行修改。

    echo "server=/#/127.0.0.1#5300" > gfwlist.conf

如果想DNS全部使用UDP转发的结果，可以删除accelerated-domains.china.conf和bogus-nxdomain.china.conf文件。

## 设置Shadowsocks

打开LuCI界面，点击Service --> Shadowsocks，填入对应的数据，Enable UDP Forward。Save & Apply。刷新后在Global Setting下的Global Server中选择刚才新建的配置。

重启

    reboot

打开手机连接该热点，就可以直接连上Google了。

## 备份

辛辛苦苦做完，但是又怕路由器出了问题也没办法像普通路由一样有个Factory Reset怎么办？可以使用`dd`指令来制作一个映像，用来保存。但是默认的会保存整个TF卡的映像，现在一个TF卡少则8G，多的几十上百，很多空间是冗余的，所以在制作映像的时候需要压缩它。

[How to shrink Raspberry Pi SD card images with GParted and dd - Knight of Pi](http://www.knight-of-pi.org/how-to-shrink-raspberry-pi-sd-card-images-with-gparted-and-dd/)

首先使用GParted查看TF卡情况，在我的这张盘上，从开始到最远的有数据的区块总大小为76MB，所以下面这条命令的最后一个count参数就设为76。注意设备名。

    sudo dd if=/dev/sdX of=/home/username/raspi_openwrt_shadowsocks_embed.img bs=1M count=76

获得的映像为76MB，可以直接`gzip raspi_openwrt_shadowsocks_embed.img`压缩一下，压缩完只有8MB不到。顺手丢进Dropbox或者别的云存储可以保存。
