---
title: 在 Windows 上使用 NFS
date: 2024-04-15 22:00:00
tags:
- Practice
categories:
- Practice
---

[NFS](https://en.wikipedia.org/wiki/Network_File_System) 协议在 Linux 上使用较广，Windows 其实也有官方的 [NFS 客户端](https://learn.microsoft.com/en-us/windows-server/storage/nfs/nfs-overview)，只是默认没有使用，这里记录下在 Windows 使用 NFS 遇到的一些问题和解决方案。

<!-- more -->

## 启用 Windows NFS 客户端

Windows 默认没有启用 NFS 客户端，需要手动启用。任务栏搜索「启用或关闭Windows功能」,然后找到「NFS服务」勾选即可启用 Windows NFS 客户端。
可执行 `showmount` 命令测试是否启用成功。

```bash
showmount -e [yourNfsServerIP]
```

启用之后就可以在文件资源管理器地址栏和使用 SMB 一样挂载 NFS 地址了，如输入 `\\10.0.0.1\share`。

也可以使用 `cmd` （注意不是 `powershell`）的 `mount` 命令进行挂载为网络盘符，详见[微软官方文档](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/mount)。

```cmd
mount -o nolock \\10.0.0.1\share N:
```

## 挂载后无写入权限

Windows 挂载 NFS 后可能发现没有写入权限，这是因为 Windows 挂载 NFS 时找不到 Windows 用户和 Unix 用户的映射关系，然后默认将用户映射为「匿名用户」。

> Access to Network File System (NFS) file servers requires UNIX-style user and group identities, which are not the same as Windows user and group identities. To enable users to access NFS shared resources, Client for NFS can retrieve UNIX-style identity data from Active Directory (if the schema includes the appropriate attributes), or from a User Name Mapping server. If Active Directory does not include UNIX-style identity attributes and a User Name Mapping server is not available on your network, then Client for NFS will attempt to access NFS resources anonymously.

匿名用户默认 `uid=-2;gid=-2` ，即 Linux 上的 `nobody` 用户（`uid=65534;gid=65534`），而 `nobody` 用户有没有 NFS 服务器上共享目录的写入权限。如需写入可以登录 NFS 服务器开启该目录的其他用户写入权限。如果没权限登录该服务器，则可以通过注册表修改 Windows 映射匿名用户的 `uid` 和 `gid` 为 `0` ，对应 Linux 的 `root` 用户（如果 NFS 服务器开启了 `root_squash` 选项，该方法会失效）。

修改方法：新建文本文档，输入以下内容并保存为 `nfs_uid0.reg`，然后双击该文件即可导入至注册表完成修改。

```reg
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ClientForNFS\CurrentVersion\Default]
"AnonymousGID"=dword:00000000
"AnonymousUID"=dword:00000000
```

导入后需重启 NFS 客户端或者重启电脑。

```cmd
nfsadmin client stop
nfsadmin client start
```

参考：https://superuser.com/questions/103970/how-to-set-identity-for-windows-client-for-nfs-without-identity-server

## 挂载后中文文件名为乱码

这是因为 Linux 文件名一般都使用 `UTF-8` 编码，而 Windows 不是，可以通过以下方式修改。

任务栏搜索「区域」（控制面板选项），或者运行 `intl.cpl`，点击「管理」选项卡 -> 更改系统区域设置 -> 勾选使用 UTF-8。

![win-utf-8](https://raw.githubusercontent.com/csJd/csJd.github.io/res/win-utf-8.png)

注：修改后可能有个别程序出现乱码情况，请根据个人需要决定是否修改。
