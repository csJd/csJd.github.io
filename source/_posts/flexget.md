---
title: FlexGet 自动 PT 下载设置
tags:
  - Practice
categories:
  - Experience
date: 2020-02-17 12:19:07
---

家里网络可连接性较差，挂的 PT 一直没有上传，就尝试在 VPS 上配置远程 PT 下载，这里记录下在 Ubuntu 18.04 VPS 上使用 FlexGet 自动下载 PT 站资源的设置。

<!-- more -->

## 前置软件包安装

### 安装 PT 下载客户端

PT 下载客户端推荐使用 [Transmission](https://transmissionbt.com) 或 [qBittorrent](https://www.qbittorrent.org/download.php)，据说 Transmission 更适合保种，qBittorrent 适合下载新/热种赚上传。

#### Transmisson

Transmisson 使用以下命令安装：

```sh
sudo apt update
sudo apt install transmission-daemon
```

安装后配置文件位于：`/etc/transmission-daemon/settings.json`，其详细配置信息可见 [官方 Wiki](https://github.com/transmission/transmission/wiki/Editing-Configuration-Files)，编辑该配置文件，主要需要修改的项目如下：

```json
{
    "rpc-authentication-required": true,
    "rpc-password": "transmission",
    "rpc-username": "transmission",
    "rpc-whitelist-enabled": false
}
```

然后执行 `service transmission-daemon start` 就可以开启 Transmission 后台服务，其 Web 管理服务的默认端口为 `9091`，浏览器输入 `http://<yourVpsIP>:9091` 就可以通过 Web 管理界面远程控制 VPS 上的 Transmission，默认提供的 Transmission RPC 界面较简陋，可以额外安装第三方开源的 [Transmission Web Control (TWC)](https://github.com/ronggang/transmission-web-control) 作为替换。进入 Web 管理界面后可自行修改其他设置选项。

#### qBittorrent

Ubuntu 官方源提供的 qBittorent 版本较旧，部分 PT 站点不支持使用旧版本，可以使用 qBittorrent 官方 PPA 安装最新的稳定版本:

```sh
sudo add-apt-repository ppa:qbittorrent-team/qbittorrent-stable
sudo apt-get update
sudo apt-get install qbittorrent-nox
```

运行 `qbittorrent-nox -d` 即在后台运行 qBittorrent，其 Web 管理服务默认端口为 `8080`，默认用户名为 `admin`，默认密码为 `adminadmin`，浏览器输入 `http://<yourVpsIP>:8080` 进入 Web 管理界面后可以修改这些内容及其他设置。

## FlexGet 设置

下载 PT 站点较新的内容可以较快获得上传量，但每次都手动添加也较麻烦，使用 [FlexGet](https://flexget.com/InstallWizard/Linux) 可以利用 PT 站点提供的 RSS 订阅实现自动下载最新的资源以及删除旧的内容。

### FlexGet 安装

FlexGet 基于 Python 实现，执行以下命令安装：

```sh
sudo apt install python3 python3-pip
pip3 install flexget
```

使用 Transmission 还需要安装 `transmission-rpc` 用于 FlexGet 和 Transmission 的交互：

```sh
pip3 install transmission-rpc
```

### FlexGet 配置

FelxGet 的配置文件位于 `~/.config/flexget/config.yml`。先看官方的 [配置指南](https://flexget.com/Configuration)，我的配置文件如下：

```yml
templates:
  disklimit:
    # 使用 free_space 插件，指定位置空余空间大于 space (单位为 MB) 时才执行任务
    # https://flexget.com/Plugins/free_space
    free_space:
      path: /home/ubuntu/downloads
      space: 10240

    # 使用 content_size 插件，只接受内容大小在 min 到 max (单位为 MB) 之间的 torrent
    # https://flexget.com/Plugins/content_size
    content_size:
      min: 1024
      max: 30720
      strict: yes

  qb:
    # qBittorent 插件配置模板
    # https://flexget.com/Plugins/qbittorrent
    qbittorrent:
      # 下载保存路径
      path: /home/ubuntu/downloads/
      # 设置下载分类
      label: rss
      host: localhost
      port: 8080
      # 设置单种最大下载/上传速度
      maxdownspeed: 30000
      maxupspeed: 10000
      # 若在 WebUI 的设置界面勾选了「对本地主机上的客户端跳过身份验证」，可省略用户名和密码
      username: admin
      password: adminadmin
  tr:
    # Transmission 插件配置模板
    # https://flexget.com/Plugins/transmission
    transmission:
      path: /home/ubuntu/downloads/
      host: localhost
      port: 9091
      username: transmission
      password: transmission

# 如果想只下载优惠内容可安装 nexusphp 插件
# https://github.com/Juszoe/flexget-nexusphp#site

# 任务设置
tasks:
  # 任务名称
  frds:
    # PT 站 RSS 地址，需要包含自己的 passkey
    rss: https://pt.keepfrds.com/torrentrss.php?rows=20&passkey=jxxx
    # url 作为标记访问过的条件
    seen:
      fields:
        - url
    # 正则表达式过滤规则
    regexp:
      accept:
        - ""
    template:
      - disklimit
      - tr

  tjupt:
    rss: https://www.tjupt.org/torrentrss.php?rows=10&passkey=xxx
    seen:
      fields:
        - url
    # 接受全部输入
    accept_all: yes
    template:
      - disklimit
      - qb
  
  # 自动清理 Transmission 旧种子和数据
  # https://flexget.com/Cookbook/TorrentCleanup
  clean:
    from_transmission:
      host: localhost
      port: 9091
      username: transmission
      password: transmission
    disable: [seen, seen_info_hash]
    if:
      - transmission_progress == 100: accept
      - transmission_secondsSeeding < 86400: reject
      - transmission_ratio < 0.5: reject
    transmission:
      host: localhost
      port: 9091
      username: transmission
      password: transmission
      action: purge

# 这里关闭 schedules 功能，后续使用 crontab 来实现定时执行
schedules: no
```

### FlexGet 测试执行

防止配置文件错误导致的错误执行，可以使用 `--test` [选项参数](https://flexget.com/CLI)来测试执行，通过其输出可以判断是不是想要执行的。

```sh
flexget --test execute --tasks frds
```

防止 FlexGet 第一次下载时下载 rss 列表中的全部种子，对 `execute` 命令使用 `--learn` [命令选项参数](https://flexget.com/CLI/execute) 将当前 rss 列表的种子标记为 `seen`：

```sh
flexget execute --learn --tasks frds
```

## 自动清理 qBittorrent 的种子和数据

FlexGet 目前还没有读取 qBittorrent 种子列表的插件，所以无法直接清理种子和下载的文件，若有此需要，可以使用 [`autoremove-torrents`](https://github.com/jerrymakesjelly/autoremove-torrents.git) 工具来实现，具体详见其 [官方文档](https://autoremove-torrents.readthedocs.io/zh_CN/latest/index.html)。我使用 qBittorrent 时配置的 config 如下：

```yml
# ~/.config/autoremove-torrents/config.yml

# 任务名称
qbtask:
  client: qbittorrent
  host: http://127.0.0.1:8080
  # 若在 WebUI 的设置界面勾选了「对本地主机上的客户端跳过身份验证」，可省略用户名和密码
  username: admin
  password: adminadmin
  strategies:
    # 删除策略名称
    rmrss:
      categories:
        - rss
      remove: create_time > 86400 or (seeding_time > 28800 and connected_leecher < 5)
    rmdead:
      categories:
        - rss
      remove: create_time > 1000 and average_downloadspeed < 5
  delete_data: true
```

在该配置文件所在目录执行 `autoremove-torrents --view` 可以测试执行而不真正删除种子及数据。

## 定时执行任务

运行 `flexget execute --task frds` 即立刻执行一次 `frds` 任务，要实现定时自动执行相关任务，可使用 `cron` 来实现。

运行 `crontab -e` 来编辑 crontab 实现 [定时执行任务](https://flexget.com/InstallWizard/Partial/Crontab)，我编辑的内容如下：

```sh
# 开机自启 qBittorrent
@reboot /usr/bin/qbittorrent-nox -d
# 每半小时执行一次 autoremove-torrents 工具，--conf 指定文件 --log 指定保存 log 文件夹
*/30  *  *  *  *  /home/ubuntu/.local/bin/autoremove-torrents --conf=/home/ubuntu/.config/autoremove-torrents/config.yml --log=/home/ubuntu/.config/autoremove-torrents

# 每半小时执行一次 clean 和 frds 任务
*/30  *  *  *  *  /home/ubuntu/.local/bin/flexget --cron execute --tasks clean frds
```

---

关于 FlexGet 更多详细用法可见 [官方 Cookbook](https://flexget.com/Cookbook)。
