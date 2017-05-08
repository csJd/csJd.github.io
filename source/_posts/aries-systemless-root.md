---
title: 小米 2s Magisk 框架实现 Systemless Root
tags:
  - Phone
categories:
  - Experience
date: 2017-05-08 16:20:20
---

听说最近出了个 Magisk 框架，可以在不修改 /system 下实现 root 和 xposed ，这样理论上装了 xposed 框架还能 OTA ，听起来真不错。就拿出了我的 2s 折腾了下，发现真的可以，这里分享一下过程。
我是卡刷官方倒数第二个稳定版（[V8.1.2.0.LXACNDI](http://bigota.d.miui.com/V8.1.2.0.LXACNDI/miui_MI2_V8.1.2.0.LXACNDI_95e24f367d_5.0.zip)），清空全部数据再进行以下操作的，不保证其他情况不会出现问题，请自行尝试。

<!-- more -->

# 1. 刷入 TWRP Recovery
在[ TWRP 官网](https://dl.twrp.me/aries/)下载小米2s最新的 TWRP Recovery ,使用 fastboot 刷入并重启进入recovery，命令如下
```
fastboot flash recovery twrp-3.1.0-0-aries.img
fastboot reboot
```
不会的可以参考[此贴](http://bbs.xiaomi.cn/t-10498119)

# 2. 下载相关文件并复制到手机
下载以下文件并复制到手机，TWRP Recovery 是支持 MTP 连接电脑的， 可以不进入系统直接在 Recovery 模式下连接电脑将相关文件复制到手机， 
* 最新版 [Magisk - zip](http://tiny.cc/latestmagisk)
* 最新版 [Magisk Uninstaller - zip](http://tiny.cc/latestuninstaller) 
* 最新版 [Magisk Manager - apk](http://tiny.cc/latestmanager)
  这里也给出 Magisk 的[ xda 发布链接](https://forum.xda-developers.com/apps/magisk/official-magisk-v7-universal-systemless-t3473445)。
* Magisk Xposed for MIUI by SolarWarez - zip，[下载链接](https://forum.xda-developers.com/attachment.php?attachmentid=3950449&d=1480267001)，[ xda 发布链接](https://forum.xda-developers.com/xposed/unofficial-xposed-miui-t3367634)
* Material Design Xposed Installer - apk，[ xda 发布链接](https://forum.xda-developers.com/xposed/material-design-xposed-installer-t3137758)

# 3. 刷入 Magisk
在 TWRP Recovery 刷入 **Magisk-v12.0.zip** ，重启进入系统，第一次重启会很慢，是正常的。进入系统后安装 **MagiskManager-v4.3.3.apk**。以上文件名中的版本均为我安装时的最新版。
打开 Magisk Manager 类似下图就是安装成功了：
![](https://raw.githubusercontent.com/csJd/csJd.github.io/res/aries-systemless-root-0.png)
Magisk 是自带 root 的，不修改 /system 的 root 。

# 4. 安装 xposed 模块
在上图 Magisk Manager 主界面左划点击模块，添加模块，选择刚下载的 **xposed-v87-sdk21-Magisk-MIUI-edition-by-SolarWarez-20161127.zip** 就行了。
左划点击下载也能安装更多自己想要的 Magisk 模块。
然后重启，也很慢，安装 **XposedInstaller_by_dvdandroid_29_04_17.apk** ，xposed 也 ok 了，如下图：
![](https://raw.githubusercontent.com/csJd/csJd.github.io/res/aries-systemless-root-1.png)

# 5. 尝试 OTA
都安装好了后，我试了下 OTA ，系统果然是会推送 OTA 的：
![](https://raw.githubusercontent.com/csJd/csJd.github.io/res/aries-systemless-root-2.png)
但要先刷入 **Magisk-uninstaller-20170320.zip**，卸载了 Magisk 才能安装 OTA。不急，安装完 OTA 重启后，再重新刷一下 **Magisk-v12.0.zip** 就行，之前安装的 Magisk 模块不用再重新装了。
