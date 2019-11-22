---
title: 在 VPS 上搭建 Shadowsocks Server
date: 2017-02-27 13:10:10
tags:
- Practice
- VPS
categories: Tutorials
---

分享在 Ubuntu 18.04 VPS 上搭建 Shadowsocks Server 的方法。

<!--more-->

## 一. 安装 Shadowsocks

执行以下命令即完成 Shadowsocks 的安装：

``` bash
sudo apt update
sudo apt install shadowsocks-libev
```

## 二. 开启 BBR

[BBR](https://github.com/google/bbr) 是 Google 提出的较新的 TCP 拥塞控制算法，已集成在了 `4.9.0` 及更新版本的 Linux 内核中，但需要手动开启。
终端输入 `sudo vi /etc/sysctl.conf` 在最后添加以下内容：

```conf
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr
```

再执行 `sudo sysctl -p` 即可开启 BBR。

## 三. 编辑 Shadowsocks 服务端的配置文件

终端输入 `sudo vi /etc/shadowsocks-libev/config.json`，修改配置文件内容如下：

``` json
{
    "server":"0.0.0.0",
    "server_port":8388,
    "local_port":1080,
    "password":"setYourPassword",
    "timeout":600,
    "method":"aes-256-gcm",
    "mode":"tcp_and_udp",
    "fast_open":true,
}
```

密码设置自己的，其余可与以上相同，其中每项的具体作用可查阅 [Shadowsocks 官网](https://shadowsocks.org/en/config/quick-guide.html)。

## 四. 开启 Shadowsocks 后台服务

执行以下命令即可开启 Shadowsocks 后台服务：

```sh
sudo service shadowsocks-libev start
```

开启服务后重启 VPS 该服务也会自动开启，以下命令可以查看服务运行状态：

```sh
sudo service shadowsocks-libev status
```

---

## Additional (not required)

* Using a well-know port (such as 443) might make it more stable
* If you have [simple-obfs](https://github.com/shadowsocks/simple-obfs) installed in your server, add following to `config.json` of `ss-server`

  ```json
  "plugin":"obfs-server",
  "plugin_opts":"obfs=tls;failover=127.0.0.1:443",
  ```

  to enalbe well-know ports binding:

  ```sh
  sudo setcap cap_net_bind_service+ep /usr/local/bin/obfs-server
  ```

* To enable `simple-obfs` in your client, set `plugin` field as the relaive path of `obfs-local` and set `plugin_opts` field as `obfs=tls;obfs-host=t.cn`

* [Optimizing](https://github.com/shadowsocks/shadowsocks/wiki/Optimizing-Shadowsocks)
