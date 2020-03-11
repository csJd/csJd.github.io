---
title: 使用 iptables 设置防火墙
tags:
  - Practice
categories:
  - Experience
date: 2020-03-11 14:59:07
---

Lightsail 的网络管理界面可以控制端口的开启和关闭，不想开放所有端口，每次服务器上安装的新的服务都需要去管理界面开启新的端口也比较麻烦，就了解了一下使用 `iptables` 直接通过 `ssh` 终端来设置防火墙。

<!-- more -->

关于 `iptables` 的相关概念可以参考 [这篇文章](https://www.howtogeek.com/177621/the-beginners-guide-to-iptables-the-linux-firewall) 以及 [Ubuntu 提供的 Wiki](https://help.ubuntu.com/community/IptablesHowTo)。

我进行的设置如下：

```sh
# 清除现有的防火墙配置
sudo iptables -F

# 允许 localhost 的 packet
sudo iptables -A INPUT -i lo -j ACCEPT
# 允许已建立连接的相关 packet
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
# 允许 22 端口的 TCP 传入连接，用于 SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
# 允许 icmp packet，用于 ping
sudo iptables -A INPUT -p icmp -j ACCEPT

# 设置默认规则
sudo iptables -P INPUT DROP  # 禁止其他传入连接
sudo iptables -P OUTPUT ACCEPT  # 允许所有传出连接
sudo iptables -P FORWARD ACCEPT  # 允许所有传出连接
```

执行 `sudo iptables -L -v` 查看当前的防火墙设置信息，我的结果如下：

```txt
Chain INPUT (policy DROP 206 packets, 25286 bytes)
 pkts bytes target     prot opt in     out     source               destination
  100 10560 ACCEPT     all  --  lo     any     anywhere             anywhere
 1330  161K ACCEPT     all  --  any    any     anywhere             anywhere             state RELATED,ESTABLISHED
    3   148 ACCEPT     tcp  --  any    any     anywhere             anywhere             tcp dpt:ssh
    1    84 ACCEPT     icmp --  any    any     anywhere             anywhere

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain OUTPUT (policy ACCEPT 440 packets, 53272 bytes)
 pkts bytes target     prot opt in     out     source               destination
```

想禁止 `ping`，则可删除 `INPUT` 下的第 4 条规则：

```sh
sudo iptables -D INPUT 4
```

这样设置的防火墙规则都是临时的，重启后就会失效，`iptables-save` 可以手动导出当前配置到 `stdout`，`iptables-restore` 可从文件手动恢复导出的配置。
安装 `iptables-persistent` 工具包可实现防火墙设置的持久化管理：

```sh
sudo apt install iptables-persistent
sudo sh -c "iptables-save > /etc/iptables/rules.v4"
```

这样重启后防火墙的配置也会保留，更新防火墙配置后仍需手动运行 `sudo sh -c "iptables-save > /etc/iptables/rules.v4"` 保存更新的配置。
