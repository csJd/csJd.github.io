---
title: 我的 Hexo 使用实践（一）
date: 2017-02-26 09:36:10
tags:
- Hexo
- Practice
categories: Experience
---

此博客是由 Github Pages 和 Hexo 搭建的，分享以下我的使用实践。
## 一. 安装 [Hexo](https://hexo.io/zh-cn/)
### 1. 安装 Node.js 和 Git
必须安装了 Node.js 和 Git 才能安装使用 Hexo，按照 [Hexo 官方提供的教程](https://hexo.io/zh-cn/docs/index.html)安装即可，Windows 用户推荐使用安装包安装，安装过程勾选添加到 PATH。

<!-- more -->

### 2. 安装 Hexo
使用 npm 一条命令就可完成 Hexo 的安装，Windows 用户推荐使用 Windows 版 Git 提供的 Git Bash 执行此命令即后续所有命令（在随意目录鼠标右键点击空白处就有 Git Bash Here）。
``` bash
npm install -g hexo-cli
```

## 二. 部署到 Github
### 1. 创建你的 Github Pages repository
在 Github Repositories 点击 New 创建新 Repository，命名为*yourGithubUsername.github.io*，yourGithubUsername 必须是你的 Github 用户名，如下图。
![](https://github.com/csJd/csJd.github.io/raw/res/My-Practice-with-Hexo.png)
点击下方 Create repository 即可完成创建。

### 2. 初始化 Hexo
在你想维护你博客的文件夹下 clone 你刚新建的 repository，使用 https 方式每次 push 都要输入密码，故推荐使用 SSH 方式（可参考此处，[Cloning with SSH URLs](https://help.github.com/articles/which-remote-url-should-i-use/#cloning-with-ssh-urls)），在[Github SSH keys 管理界面](Github SSH keys 管理界面)上传你的 PC 的 SSH key 后，就可以使用如下命令 clone。
``` bash
git clone git@github.com:csJd/csJd.github.io.git
```
clone 完成后进入 *yourGithubUsername.github.io* 文件夹，执行以下命令初始化 Hexo
``` bash
hexo init
npm install
npm install hexo-deployer-git --save
```
### 3. 部署到 Github
进入 *yourGithubUsername.github.io* 文件夹，编辑站点配置文件 **_config.yml**，最下 deploy 处改为如下，username 替换为你的 GitHub 用户名。
``` yaml
deploy:
  type: git
  repository: git@github.com:username/username.github.io.git
  branch: master
```
改好后输入以下命令即可部署到 GitHub
``` bash
hexo g -d
```
稍等一会，访问 <https://yourGithubUsername.github.io>，就能看到 Hexo 的初始页面。
关于 `_config.yml` 的详细配置可参考[官方文档](https://hexo.io/zh-cn/docs/configuration.html)。
