---
title: 给本地项目添加Github Repo
date: 2017-03-05 19:17:56
tags:
- Github
- Git
categories:
- Experience
---
将本地项目添加Github repo后可以方便的在其它设备继续进行项目的开发，也可以方便的与其他人分享自己的项目，我这里记录一下给本地项目添加Github repo的方法。
<!-- more -->

首先要保证本地电脑安装了Git，且本地电脑的ssh公钥添加到了你的[Github账号的SSH keys](https://github.com/settings/keys)，添加公钥方法可见[GitHub给的说明](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)。然后在Github上新建一个Repo，这个新建的Repo就是用来做本地项目Github repo的，接下来就是把本地项目push到Github repo了。

## 方法一：Clone
若项目本来不属于一个本地Git repo，直接执行以下命令克隆刚新建的Github repo，再将项目文件移入就行了：
``` bash
# username为用户名，gitRepo为刚新建的Repo名称
git clone git@github.com:username/gitRepo.git
```
然后就能add，commit，push了。

## 方法二：手动添加remote
项目本身就属于一个本地Git repo时，手动添加remote比较好，在项目目录执行以下命令：
``` bash
# 给本地Git repo添加remote repo
git remote add origin git@github.com:username/gitRepo.git 
# 设置push的upstream
git push -u origin master
# 将项目push到GitHub repo
git push -f
```
这样就完成本地项目和GitHub repo的关联了。
