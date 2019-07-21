---
title: 我的 Hexo 使用实践（二）
date: 2017-02-26 13:36:10
tags:
- Hexo
- Practice
categories: Experience
---

将 Blog 成功搭到 Github 后就要考虑一些配置问题了，需要更加美观的主题，换了电脑或者重装系统后还需要快速的从以前的配置恢复，下面就解决这些问题。
<!-- more -->

## 一. 设置[主题](https://hexo.io/themes/)
Hexo 官网就提供了很多[主题](https://hexo.io/themes/)，我是用的是[NexT 主题](http://theme-next.iissnan.com/)，简单大方且不失美观，以下以 NexT 主题为例，其余主题也类似。

### 1. 下载 NexT
推荐使用[最新稳定版本](https://github.com/iissnan/hexo-theme-next/releases),下载后解压到你的站点目录下的 `themes` 目录下，并将 解压后的文件夹名称（hexo-theme-next-5.0.1）更改为  next。

### 2. 使用 NexT
下载并重命名后将站点配置文件（站点目录下的 **_config.yml** 文件）中的 theme 属性修改为 next，再次执行 `hexo g -d`，Blog 就应用新主题了。
在站点目录下的 `themes\next` 目录下也有 **_config.yml** 文件，是 NexT 主题的主题配置文件，关于其详细配置和问题可参考[官方文档](http://theme-next.iissnan.com/getting-started.html#theme-settings)和官方 [Github Wiki](https://github.com/iissnan/hexo-theme-next/wiki/%E5%88%9B%E5%BB%BA%E5%88%86%E7%B1%BB%E9%A1%B5%E9%9D%A2)。

## 二. 配置信息同步
可以发现，`hexo d` 只将站点显示所需文件 push 到了 github master 分支，markdown 源文件以及 hexo 配置文件等都只在本地，我们在系统重装或者换电脑后再次搭建就失去了原来的配置以及源文件。要想多端同步和重装后快速恢复，比较好的解决方法就是将这些源文件和配置文件也上传到 Github Repo，刚好 `hexo g` 已经提供了 **.gitignore** 文件，就是用于我们把整个站点 push 到 GitHub 上去的。
### 1. 建立 hexo 分支
由于 GitHub 上 master 分支管理的是站点显示文件，这是由 `hexo d` 命令来部署的，我们需要再建一个 hexo 分支来管理 Hexo 的配置文件和站点源文件，我们手动管理这个分支。
执行以下命令就完成了新分支的创建：
``` bash
git branch hexo     #本地新建hexo分支
git checkout hexo   #切换到hexo
git add .           #添加站点文件到git
git commit -m "add hexo branch"     #提交修改
git push origin hexo    #push本地更改到GitHub Repo
```
此时在 Github 上就看到了新的分支 hexo，在 Github 上修改 Default branch 为 hexo 分支，这样我们以后的手动 push 默认就是 hexo 分支了，正是我们所想要的。
类似的，还可以建立 res 分支存放图片等资源文件，这样就不用把这些文件放 `source` 文件夹中了，clone 的内容也更少了。像上篇文章的图片地址为 <https://github.com/csJd/csJd.github.io/raw/res/My-Practice-with-Hexo.png>，此图片就放在 res 分支下。

### 2. 恢复配置
以后在新的 PC 获重装系统后要编辑博客时只用安装 git 和 node.js，然后 clone 你的 Github Repo：
``` bash
git clone git@github.com:csJd/csJd.github.io.git
```
进入站点文件夹（*yourGithubUserName.github.io*文件夹），输入以下命令就恢复以前的环境了：
``` bash
npm install hexo-cli -g               #安装hexo
npm install                           #安装模块到node_modules
```
然后就可以像以前一样在 `source/_posts` 文件夹编写博客，使用 `hexo g -d` 即可发布到 GitHub Pages。
