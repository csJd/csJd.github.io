---
title: TCPCopy
date: 2022-12-21 15:16:56
tags:
- Practice
categories:
- Practice
---

[TCPCopy](https://github.com/session-replay-tools/tcpcopy) 是一个开源的 TCP 服务器测试工具，
功能是复制在线 server 的 TCP 类型的请求数据包，修改 TCP/IP 头部信息，
发送给测试服务器，达到欺骗测试服务器的 TCP 程序的目的。

<!-- more -->

## 测试服务器安装并启动 intercept

```bash
# 编译安装
yum install -y libnetfilter_queue-devel
wget https://github.com/session-replay-tools/intercept/archive/refs/tags/1.0.0.tar.gz
tar -xvf intercept-1.0.0.tar.gz
cd intercept-1.0.0
./configure --single --traditional --nfqueue --prefix=/opt/tcpcopy
make install

# 启动
iptables --insert OUTPUT -p tcp --sport 80 -j NFQUEUE
/opt/tcpcopy/sbin/intercept -d

# 停止
pkill -9 intercept
iptables --delete OUTPUT -p tcp --sport 80 -j NFQUEUE
# 一定要先停止现网的 tcpcopy 再停止测试机的 intercept
```

## 现网服务器安装并启动 tcpcopy

```bash
# 编译安装
wget https://github.com/session-replay-tools/tcpcopy/archive/refs/tags/v1.2.0.tar.gz
tar -xvf tcpcopy-1.2.0.tar.gz
cd tcpcopy-1.2.0
./configure --single --with-cc-opt=-O3 --prefix=/opt/tcpcopy
make install

# 启动
/opt/tcpcopy/sbin/tcpcopy -x 0:80-1.1.1.1:80 -s 1.1.1.1 -n 1 -d
# 参数说明：
# -x: transfer, src_port-dst_ip:dst_port
# -s: 运行 intercept 的 ip，例子中为 1.1.1.1
# -n: 流量放大倍数，默认为 1，即不放大
# -d: run as daemon

# 停止
pkill -9 tcpcopy
```
