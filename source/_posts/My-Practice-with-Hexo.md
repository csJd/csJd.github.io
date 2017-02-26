---
title: 我的Hexo使用实践（一）
date: 2017-02-26 09:36:10
tags:
- Hexo
- Practice
categories: Experience
---

此博客是由Github Pages和Hexo搭建的，分享以下我的使用实践。
## 一. 安装[Hexo](https://hexo.io/zh-cn/)
### 1. 安装Node.js和Git
必须安装了Node.js和Git才能安装使用Hexo，按照[Hexo官方提供的教程](https://hexo.io/zh-cn/docs/index.html)安装即可，Windows用户推荐使用安装包安装，安装过程勾选添加到PATH。

### 2. 安装Hexo
使用npm一条命令就可完成Hexo的安装，Windwows用户推荐直接使用Windwows提供的PowerShell执行此命令。
``` bash
npm install -g hexo-cli
```

## 二. 部署到Github
### 1. 创建你的Github Pages repository
在Github Repositories点击New创建新Repository，命名为`yourGithubUserName.github.io`,`yourGithubUserName`必须是你的Github用户名，如下图。
![](https://github.com/csJd/csJd.github.io/raw/res/My-Practice-with-Hexo.png)
点击下方 Create repository 即可完成创建。

### 2. 初始化Hexo
在你想维护你博客的文件夹下clone你刚新建的repository，推荐使用SSH方式（可参考此处，[Cloning with SSH URLs](https://help.github.com/articles/which-remote-url-should-i-use/#cloning-with-ssh-urls)），使用如下命令clone。
``` bash
git clone git@github.com:csJd/csJd.github.io.git
```
clone完成后进入`yourGithubUserName.github.io`文件夹，执行以下命令初始化Hexo
``` bash
hexo init
npm install
npm install hexo-deployer-git
```
### 3. 部署到Github
进入`yourGithubUserName.github.io`文件夹，编辑站点配置文件`_config.yml`，最下deploy处改为如下，username替换为你的GitHub用户名。
``` yaml
deploy:
  type: git
  repository: git@github.com:username/username.github.io.git
  branch: master
```
改好后输入以下命令即可部署到GitHub
``` bash
hexo g -d
```
稍等一会，访问 <https://yourGithubUserName.github.io>，就能看到Hexo的初始页面。
关于`_config.yml`的详细配置可参考[官方文档](https://hexo.io/zh-cn/docs/configuration.html)。
