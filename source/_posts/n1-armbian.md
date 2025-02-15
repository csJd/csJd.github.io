---
title: 在 Phicomm N1 盒子上使用 Armbian
date: 2024-05-10 23:00:00
tags:
- N1
- Armbian
categories:
- Experience
---

[Armbian](https://www.armbian.com/) 是用于单板机（SBCs）的轻量级 Debian/Ubuntu 系统，可以把电视盒子等设备变为一个小型 Linux 服务器。这里记录下在 N1 盒子上使用 Armbian 的过程。

<!-- more -->

## 下载和写入镜像到 U 盘

N1 盒子的 CPU 是 `Amlogic S905D`，在 `Armbian` 官网有 [社区维护的 Armbian 可以下载](https://www.armbian.com/amlogic-s9xx-tv-box/)。不需要图形界面选择 Minimal 或者 CLI 版本下载就可以，目前最新的 `Armbian Noble` (`Ubuntu 24.04`) 和 `Armbian Bookworm` (`Debian 12`)，个人更习惯使用 Ubuntu 的版本。

下载后使用 [`usbimager`](https://gitlab.com/bztsrc/usbimager/) 工具即可写入镜像到 U盘。

## 编辑对应配置参数

由于该镜像是多款电视盒子通用的，写入镜像后，还需执行以下操作：

### 编辑 U 盘的 `/boot/extlinux/extlinux.conf` 文件

在 `append` 行前面插入对应设备的 dtb 文件路径

```conf
#/boot/extlinux/extlinux.conf 
label Armbian_community
  kernel /Image
  initrd /uInitrd
  fdtdir /dtb/
  # 插入 n1 的 dtb 路径，dtb 文件已包含在镜像中
  FDT /dtb/amlogic/meson-gxl-s905d-phicomm-n1.dtb

  # append 行的内容不要修改
```

### 复制 CPU 对应的 u-boot 文件到 `/boot` 目录

对于 N1 也就是复制 `/boot/u-boot-s905x2-s912` 文件为 `/boot/u-boot.ext`

> 参考：[官方安装指南](https://forum.armbian.com/topic/33676-installation-instructions-for-tv-boxes-with-amlogic-cpus)

## 从 U 盘启动 armbian

以上内容修改好后将U盘插到 N1 远离 HDMI 接口的那个 USB 接口，然后通电，N1 就会从U盘启动 Armbian，首次开机会比较慢（3分钟左右），如有显示器也可以使用 HDMI 线连接显示器设置。没有显示器也可以根据路由器的设备列表查看 IP，然后 ssh 登录设置，使用 ssh 登录时，默认 root 密码为 1234。

[参见官方文档：How to login?](https://docs.armbian.com/User-Guide_Getting-Started/#how-to-login)

如果盒子没有刷入小钢炮等系统还可以执行 `/root/install-aml.sh` 将 Armbian 写入到内置 emmc 存储，这样就不用插 U 盘了，启动速度上也会快一些。个人是使用 U 盘启动的，这样不影响折腾其他系统。如果刷入了小钢炮等系统可能需要先刷回 Android 系统才能写入 emmc，刷回方法可参考[论坛教程](https://www.right.com.cn/forum/thread-413863-1-1.html)。
