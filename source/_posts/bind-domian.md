---
title: 绑定 Github Pages 到自己的域名
date: 2017-02-25 20:22:53
categories: 
- Experience
tags:
- Github
- Domain
- Hexo
---

Github提供了GitHub pages绑定到域名的方法，刚好之前申请了一个[freenom](http://www.freenom.com/zh/index.html?lang=zh)的免费域名，用自己的域名有时候会方便一些，下面是方法。

## 1. Ping
`ping`你的Github Pages对应的服务器IP
``` bash
ping csJd.github.io
```

<!-- more -->

## 2. 添加域名DNS记录
以下两方法选一种即可
* 进入你的域名管理界面，添加一个A记录到你刚`ping`得的IP，如我添加 *deng.cf* 到 *151.101.100.133*
* 喜欢用二级域名的话，可以添加一个*CNAME*记录到你的Github pages地址，如添加 *blog.deng.cf* 到 *csJd.github.io*，如下图
![](https://github.com/csJd/csJd.github.io/raw/res/Bind-github-pages-with-your-domain2.png)

## 3. 设置CNAME
在你的*yourusername.github.io*项目点Settings，在Custom domain处填上你刚添加的记录，如 *blog.deng.cf* ，这样就完成了域名的绑定，如下图
![](https://github.com/csJd/csJd.github.io/raw/res/Bind-github-pages-with-your-domain.png)
若是使用Hexo搭建，还需在*source*文件夹下新建**CNAME**文件，无后缀，内容为你上面填的域名，如*blog.deng.cf*，然后执行`hexo g -d`部署即可生效
