---
title: 在vps上搭建Shadowsocks Server的实践
date: 2017-02-27 13:10:10
tags:
- Shadowsocks
- Practice
- VPS
categories: Tutorials
---

分享在Ubuntu VPS上搭建Shadowsocks的方法。

## 一. 安装Shadowsocks
执行以下命令即完成Shadowsocks的安装：
``` bash
sudo apt install python-pip
sudo pip install shadowsocks
```

<!--more-->
## 二. 编辑Shadowsocks服务端的配置文件
在用户主目录新建sss.json文件作为Shadowsocks Server的配置文件，`vi ~/sss.json`
配置文件内容如下：
``` json
{
    "server":"0.0.0.0",
    "server_port":8388,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"setYourPassword",
    "timeout":300,
    "method":"rc4-md5",
    "fast_open":false
}
```
密码设置自己的，其余可与以上相同，其中每项的具体作用可查阅[Shadowsocks官网](https://shadowsocks.org/en/config/quick-guide.html)。

## 三. 开启Shadowsocks后台服务
执行以下命令即可开启Shadowsocks后台服务：
``` bash
sudo ssserver -c ~/sss.json -d start
```
同样的，关闭Shadowsocks后台服务则执行以下命令：
``` bash
sudo ssserver -c ~/sss.json -d stop
```

