---
title: 使用 Travis CI 自动构建 Hexo 博客
date: 2018-02-16 13:36:10
tags:
- Hexo
- Practice
categories: Experience
---

[Travis CI](https://travis-ci.com/) 提供的持续集成服务可以在我们将 Commit push 到 Github 后自动执行用户定义的一系列任务。这里记录一下使用 Travis CI 对 Hexo 静态博客自动部署到 Github Pages 的过程。

<!--more-->

## 设置 Travis CI

使用 GitHub 登录 [Travis CI](https://travis-ci.com/)，会自动读取 GitHub repositories 信息。Travis CI 的原理是在虚拟机 clone 对应的 GitHub repository 并进入该目录下执行用户在配置文件中定义的一些过程。而 Travis CI 的虚拟机默认是不具备我们 GitHub Pages 对应的 repository 的写权限的，所以需要提供 Access token，这里在 [GitHub 设置](https://github.com/settings/tokens) 里点击「Generate new token」建一个 Access token，权限只需要选择 `public_repo` 即可。然后将该 token 添加到 Travis CI 对应 repo 设置里的「Environment Variables」处，如下图：
![image](https://raw.githubusercontent.com/csJd/res/master/pic/image.ieihyyw7zge.png)
`NAME` 处填 `GITHUB_TOKEN`， `VALUE` 处填刚才复制的 token，点击右方「Add」就可以添加环境变量 `GITHUB_TOKEN`，这个在后面会用到

## 编辑 `.travis.yml`

在 Hexo 站点源文件根目录新建 `.travis.yml` 文件，比如我的 Hexo 站点源文件托管在 [个人 GitHub Pages repo](https://github.com/csJd/csJd.github.io) 的 `hexo` branch，而生成的静态博客内容在 `master` branch，则设置 `.travis.yml` 文件内容如下：

```yml
# Travis CI for hexo on GitHub Pages

language: node_js
node_js: stable

# Build on which branches
branches:
  only:
    - hexo

install:
  # install pandoc which is required by `hexo-renderer-pandoc`
  # - wget https://github.com/jgm/pandoc/releases/download/2.7.3/pandoc-2.7.3-1-amd64.deb
  # - sudo dpkg -i pandoc-2.7.3-1-amd64.deb
  # `npm ci` is better than `npm install` on CI
  - npm ci

script:
  - hexo g

# deply to GitHub Pages https://docs.travis-ci.com/user/deployment/pages/
deploy:
  provider: pages
  local_dir: public/
  target_branch: master  # branch for GitHub Page
  skip_cleanup: true
  committer_from_gh: true
  # Set in the settings page of your repository, as a secure variable
  github_token: $GITHUB_TOKEN
  keep_history: false
  on:
    branch: hexo  # branch for the source files
```

## Push to GitHub

编辑好 `.travis.yml` 文件后，push 到 GitHub，Travis CI 就会自动触发构建过程，相当于我们本地执行了 `hexo g -d`。后续每次只用编辑 blog 的 markdown 源文件，每次 push 到 GitHub，Travis CI 都会帮我们完成构建过程，跨设备编辑也变得更加方便，本机都可以不用安装 `hexo`，`node.js` 之类的了。
Travis CI 构建成功的 [状态](https://travis-ci.com/csJd/csJd.github.io) 如下图：
![image](https://raw.githubusercontent.com/csJd/res/master/pic/image.qg9k3370t5a.png)
