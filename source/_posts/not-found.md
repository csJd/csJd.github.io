---
title: 在 VPS 上搭建 NOT FOUND Server
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

再执行 `sudo sysctl -p` 即可开启 BBR，执行 `lsmod | grep bbr` 可查看是否开启。

## 三. 编辑 Shadowsocks 服务端的配置文件

终端输入 `sudo vi /etc/shadowsocks-libev/config.json`，修改配置文件内容如下：

``` json
{
    "server":"0.0.0.0",
    "server_port":8388,
    "password":"setYourPassword",
    "timeout":600,
    "method":"aes-256-gcm",
    "mode":"tcp_only",
    "fast_open":false,
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

* Using a well-know port (such as 80, 443) might make it more stable
* If you have [v2ray-plugin](https://github.com/shadowsocks/v2ray-plugin) installed in your server (build or download the compatible version and place it to `/usr/local/bin/`), add following to `config.json` of `ss-server` to enable it

  ```json
  "plugin":"v2ray-plugin",
  "plugin_opts":"server",
  ```

  to enalbe well-know ports binding:

  ```sh
  sudo setcap cap_net_bind_service+ep /usr/local/bin/v2ray-plugin
  ```

* To enable `v2ray-plugin` in your client:
  * set `plugin` field as the relative path of `v2ray-plugin`
  * set `plugin_opts` field as what you want, keep it empty is ok

* [Optimizing](https://github.com/shadowsocks/shadowsocks/wiki/Optimizing-Shadowsocks)
  Create `/etc/sysctl.d/local.conf` with the following content:

  ```conf
  # max open files
  # fs.file-max = 51200
  # max read buffer
  net.core.rmem_max = 67108864
  # max write buffer
  net.core.wmem_max = 67108864
  # default read buffer
  # default read buffer
  net.core.rmem_default = 65536
  # default write buffer
  net.core.wmem_default = 65536
  # max processor input queue
  net.core.netdev_max_backlog = 4096
  # max backlog
  net.core.somaxconn = 4096

  # resist SYN flood attacks
  net.ipv4.tcp_syncookies = 1
  # reuse timewait sockets when safe
  net.ipv4.tcp_tw_reuse = 1
  # turn off fast timewait sockets recycling
  net.ipv4.tcp_tw_recycle = 0
  # short FIN timeout
  net.ipv4.tcp_fin_timeout = 30
  # short keepalive time
  net.ipv4.tcp_keepalive_time = 1200
  # outbound port range
  net.ipv4.ip_local_port_range = 10000 65000
  # max SYN backlog
  net.ipv4.tcp_max_syn_backlog = 4096
  # max timewait sockets held by system simultaneously
  net.ipv4.tcp_max_tw_buckets = 5000
  # turn on TCP Fast Open on both client and server side
  net.ipv4.tcp_fastopen = 3
  # TCP receive buffer
  net.ipv4.tcp_rmem = 4096 87380 67108864
  # TCP write buffer
  net.ipv4.tcp_wmem = 4096 65536 67108864
  # turn on path MTU discovery
  net.ipv4.tcp_mtu_probing = 1
  ```

  then run:

  ```sh
  sysctl --system
  ```
