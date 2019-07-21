---
title: AWS Educate & Amazon Lightsail
tags:
  - Practice
categories:
  - Practice
date: 2019-07-20 22:19:31
---

之前申请的 DigitalOcean 的云主机最近到期了，偶然看到学校已经在 [AWS Educate](https://aws.amazon.com/cn/education/awseducate/) 的 [会员院校列表](https://s3.amazonaws.com/awseducate-list/AWS_Educate_Institutions.pdf) 里了，注册 AWS Educate 后可以得到 $100/年 的服务抵扣金额（Credits），配合 AWS 的 [Amazon Lightsail](https://lightsail.aws.amazon.com/ls/webapp/home/instances) 实例可以使用一年多，趁着还是学生，决定折腾一个，这里记录一下过程。

<!--more-->

# 注册 AWS Educate
在 [AWS Educate 主页](https://aws.amazon.com/cn/education/awseducate) 点击 「加入 AWS Educate」 ，身份选择 「Student」，下一步，电子邮件需要填学校的邮箱，收到邮件点击链接就可以完成注册。有信用卡（别人的没用过的也行）的话就继续注册 AWS 账户，没有就直接使用 AWS Educate Starter 账户，但是会有 [一些限制](https://aws.amazon.com/cn/premiumsupport/knowledge-center/educate-starter-account/)，送的抵扣金额也少一些。我注册的是 AWS 账户，以下步骤也是针对 AWS 账户，AWS Educate Starter 账户可能会有所不同。

# 注册 AWS 账户（需信用卡）
注册 AWS 账户需要信用卡，银联的信用卡也是可以的。在 AWS [账户注册站点](https://portal.aws.amazon.com/billing/signup#/start) 按步骤即可完成注册。注册完成后再 “我的账户” 可以看到账户 ID（12位数字），复制账户 ID，再登录 [AWS Educate](https://www.awseducate.com/signin/SiteLogin) 输入 AWS 账户 ID 就可以得到 $100 的服务抵扣金额的优惠代码，可在 [此处](https://www.awseducate.com/student/s/awssite) 查看。

![image](https://raw.githubusercontent.com/csJd/res/master/pic/image.png)

# 使用 Amazon Lightsail
[Amazon Lightsail](https://lightsail.aws.amazon.com/ls/webapp/home/instances) 类似于 DigitalOcean 之类的 VPS 服务商家，比起之前用过的 EC2 实例要实惠多了，而且可以使用 AWS Credits，每月有 1T 的流量，最低配置只需要 $3.5/月，用户体验极棒。

## 新建实例
在 [新建实例](https://lightsail.aws.amazon.com/ls/webapp/home/instances) 的地方点击 「Create Instance」 按步骤即可完成实例的新建，记得区域一定选东京（Tokyo），系统选 Ubuntu 18.04，具体如下图：

![image](https://raw.githubusercontent.com/csJd/res/master/pic/image.se1sl2h6eqp.png)

Plan 选择最低的 $3.5/月 就够用了，点击最下方 「Create instalce」 就可以完成实例的创建，然后就拥有一个在 Tokyo 的云主机啦。


## 绑定静态 IP
Amazon Lightsail 默认是动态 IP，可以手动绑定静态 IP，在 [创建静态 IP](https://lightsail.aws.amazon.com/ls/webapp/create/static-ip?region=ap-northeast-1)  的页面即可绑定，绑定之后的静态 IP 是免费的，如下图：

![image](https://raw.githubusercontent.com/csJd/res/master/pic/image.ajgxeezmehm.png)

点击最下方的 「Create」 即可完成静态 IP 的创建和绑定，就可以使用 SSH 通过该静态 IP 来访问刚新建的 LightSail 实例了。

## SSH 连接
Lightsail 默认只能使用 SSH key 来连接，可以在 [这里下载](https://lightsail.aws.amazon.com/ls/webapp/account/keys) 用于连接的 SSH key，进入刚创建的 Lightsail 实例的 [详情界面](https://lightsail.aws.amazon.com/ls/webapp/ap-northeast-1/instances/Ubuntu-1/connect)，复制实例的 IP，通过该 IP 和刚下载的 SSH key，就可以连接该实例了.
```sh
# 需要修改 SSH key 的权限为 400，仅第一次需要修改
chmod 400 LightsailDefaultKey-ap-northeast-1.pem
# ssh 连接
ssh -i LightsailDefaultKey-ap-northeast-1.pem ubuntu@[your_ip_address]
```
成功连接：

![image](https://raw.githubusercontent.com/csJd/res/master/pic/image.w5465qhk2r.png)

## 端口开放
新建的 Lightsail 实例默认只开启了用于 SSH 连接的 22 端口，如需使用其他端口可在实例详情界面的 「Networking」 的 「Firewall」 手动开启相关端口，不清楚具体开启什么端口可以在 「Application」处选择「All TCP + UDP」开启所有端口。

---
然后就可以使用啦~

