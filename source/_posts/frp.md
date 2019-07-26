---
title: FRP 内网穿透应用 - SSH
tags:
  - Practice
categories:
  - Practice
date: 2019-06-06 15:33:31
---

学校 PC 没有独立公网 IP， 这样在学校外部网络就无法 SSH 连接。如果有某台服务器（比如用来搭建博客的服务器，阿里云等）是有公网 IP 的，就可以利用 [frp](https://github.com/fatedier/frp) 和这台服务器来实现内网穿透，来达到校外通过 SSH 访问学校内网 PC 的目的。

<!--more-->

# frp 服务器配置

## 下载 frp
在有公网 IP 的服务器上，下载 [frp 最新 release](https://github.com/fatedier/frp/releases)，并解压到 `/opt`
```sh
wegt https://github.com/fatedier/frp/releases/download/v0.27.0/frp_0.27.0_linux_amd64.tar.gz
sudo tar -xvf frp_0.27.0_linux_amd64.tar.gz -C /opt
sudo mv /opt/frp_0.27.0_linux_amd64 /opt/frp
```

## 启用 frps 服务
修改 `/opt/frp/systemd/frps.service` 中的文件路径 (第 10 行) 为
```sh
ExecStart=/opt/frp/frps -c /opt/frp/frps.ini
```

然后执行以下命令，启用 frps 服务
```sh
sudo cp /opt/frp/systemd/frps.service /etc/systemd/system/
sudo service frps start
```
这样就完成了服务器端的配置。


# frp 客户端配置
客户端即你想在校外访问的 PC ，配置方法与服务器端配置类似，注意以下操作都是在客户端进行。

## 内网 PC 下载 frp
参见 [frp 服务器配置](#frp-%E5%AE%A2%E6%88%B7%E7%AB%AF%E9%85%8D%E7%BD%AE) 中的 [下载 frp](#%E4%B8%8B%E8%BD%BD-frp)

## 修改 frpc.ini
修改 `/opt/frp/frpc.ini` 如下
```ini
[common]
server_addr = 1.1.1.1
server_port = 7000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 22000
```
其中 `server_addr` 为你的服务器公网 ip 或者绑定的域名，也可以使用其他人提供的 [免费 frp 服务器](http://www.frps.top/)。
`remote_port` 为端口号，可设置为任何不冲突端口号（如 22000），后续外网访问内网 PC 就是使用此端口号。


## 启用 frpc 服务
修改 `/opt/frp/systemd/frpc.service` 中的文件路径 (第 10，11 行) 为
```service
ExecStart=/opt/frp/frpc -c /opt/frp/frpc.ini
ExecReload=/opt/frp/frpc reload -c /opt/frp/frpc.ini
```
然后执行以下命令，启用 frpc 服务
```sh
sudo cp /opt/frp/systemd/frps.service /etc/systemd/system/
sudo service frps start
```
这样就完成了客户端的配置。

# 外网访问
现在就可以使用 SSH 外网连接内网 PC 了，端口号和上面配置的相对应就行。
```sh
ssh deng@1.1.1.1 -P 22000
```
---
更多 frp 的相关应用可以查看 [官方文档](https://github.com/fatedier/frp/blob/master/README_zh.md)。
