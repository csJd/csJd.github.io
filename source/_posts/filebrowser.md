---
title: 使用 Filebrowser 分享文件 
date: 2023-03-01 15:16:56
tags:
- Practice
categories:
- Practice
---

[Filebrowser](https://github.com/filebrowser/filebrowser) 是一个开源的服务器文件管理和分享工具
可以较方便的实现服务器上文件的分享，这里记录下个人的使用经验

<!-- more -->

## 安装和配置

一行代码就可以完成安装

```bash
# https://filebrowser.org/installation
curl -fsSL https://raw.githubusercontent.com/filebrowser/get/master/get.sh | bash
```

初始化配置

```bash
# 初始化配置并保存至当前目录的 `filebrowser.db`
filebrowser config init \
  --address 0.0.0.0 \
  --port 443 \
  --root ~/downloads/
  --cert ~/ssl/domain_cert.cer \
  --key ~/ssl/domain_key.key \

# 参数说明：https://filebrowser.org/cli/filebrowser-config-init
# --address, --port 监听地址与端口
# --root 待分享文件的根目录
# --cert, --key TLS 证书和 key, 用于启用 https 访问，不配置则为 http 访问

# 添加用户
filebrowser users add {username} {password}
```

然后启动程序，就可以通过浏览器访问 filebrowser (https://your_domain.com)

```bash
filebrowser -d filebrowser.db
```

## 创建 systemd 服务
filebrowser 自身没有作为 daemon 运行的选项，可以自己创建一个 [systemd 服务](https://www.freedesktop.org/software/systemd/man/systemd.service.html)，
从而方便开机启动和服务重启等

创建用户级的 systemd 服务

```conf
# vim ~/.config/systemd/user/filebrowser.service
[Unit]
Description=Filebrowser
After=network-online.target

[Service]
ExecStart=/usr/local/bin/filebrowser -d /home/ubuntu/filebrowser.db

[Install]
# Here must be `default.target`, the service otherwise won't start on boot
WantedBy=default.target
```

```sh
# systemd reload 以识别刚新建的服务
systemctl --user daemon-reload

# 启动服务
systemctl --user start filebrowser  
systemctl --user status filebrowser

# 开机自动启动服务
systemctl --user enable filebrowser
```

[systemd service handbook](https://linuxhandbook.com/create-systemd-services)
