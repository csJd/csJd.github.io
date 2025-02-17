---
title: 使用 acme.sh 自动申请和续期 SSL 证书
tags:
  - acme.sh
  - SSL
categories:
  - Experience
date: 2025-02-16 09:46:23
---

现在申请的免费 SSL 证书一般只有 3 个月，每次证书过期都手动更新比较麻烦，使用 [acme.sh](https://github.com/acmesh-official/acme.sh) 可以比较方便的进行证书申请、续期和部署。

<!-- more -->

## 安装 acme.sh

acme.sh 申请证书默认使用 [ZeroSSL](https://app.zerossl.com/certificates) CA，可以先使用邮箱注册 ZeroSSL 账号，安装时需提供该邮箱。
如需使用其他 CA 可参考 [官方文档](https://github.com/acmesh-official/acme.sh/wiki/Server)。

```sh
curl https://get.acme.sh | sh -s email=xx@gmail.com
# acme.sh  --register-account --server zerossl # 注册 ZeroSSL 账号
```

## 申请证书

acme.sh 需要验证用户对域名的所有权才能对该域名申请证书，推荐将域名托管到 [Cloudflare](https://www.cloudflare.com/)，即在域名注册处将域名的 DNS 服务器修改为 Cloudflare 分配的域名服务器（[Namecheap 流程](https://www.namecheap.com/support/knowledgebase/article.aspx/9607/2210/how-to-set-up-dns-records-for-your-domain-in-a-cloudflare-account/)，[腾讯云流程](https://cloud.tencent.com/document/product/302/5518)）。

在 CF 上配置好域名后可以在 CF 个人 Profile 页面生成用于更新 DNS 的 API token（Create Token -> Edit zone DNS -> Zone Resources 选择 All zones -> Continue），将 `CF_Token` 和 `CF_Account_ID` 配置为环境变量后就可以使用 Cloudflare 的 API 自动验证域名和申请证书。详细可参考 [官方文档](https://github.com/acmesh-official/acme.sh/wiki/dnsapi#dns_cf)。

```sh
# https://dash.cloudflare.com/profile/api-tokens
export CF_Token="xxx"
export CF_Account_ID="xxx"
acme.sh --issue --dns dns_cf -d xx.deng.im
```

## 安装证书

申请的证书文件 acme.sh 会存档到 `~/.acme.sh/` 下对应域名的管理目录，并定期自动续期证书（默认为 60 天），如需自动部署续期的证书到相关服务，还需要安装证书。安装证书不要直接使用/复试 `~/.acme.sh/`  下的证书文件，而是用 acme.sh 提供的 `install-cert` 命令。

> After the cert is generated, you probably want to install/copy the cert to your Apache/Nginx or other servers.You **MUST** use this command to copy the certs to the target files, **DO NOT** use the certs files in `~/.acme.sh/` folder, they are for internal use only, the folder structure may change in the future.

```sh
# 将证书安装到 ~/ssl/domain/ 目录，安装后 reload nginx 服务使新的证书生效
acme.sh --install-cert -d xx.deng.im \
--key-file ~/ssl/xx.deng.im/key.crt \
--fullchain-file ~/ssl/xx.deng.im/fullchain.crt \
--reloadcmd "service nginx force-reload"
```

## MISC

```sh
acme.sh --list  # 查看 acme 管理的域名列表
acme.sh --info -d domain.om  # 查看域名的详细信息
acme.sh --upgrade  # 更新 acme.sh
```
