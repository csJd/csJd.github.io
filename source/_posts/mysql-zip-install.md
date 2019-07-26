---
title: Windows 上使用 zip 包安装 MySQL
date: 2017-03-07 22:14:30
tags:
- MySQL
categories:
- Experience
---

由于重装系统比较频繁，每次都使用安装包安装 MySQL 的话比较麻烦，看到 [MySQL 官网](https://dev.mysql.com/downloads/mysql/)提供了 MySQL 的 ZIP Archive，查了下[官方文档](https://dev.mysql.com/doc/refman/5.7/en/windows-install-archive.html)，这里记录一下使用 ZIP 包安装 MySQL 的方法。

<!-- more -->

## 1. 解压，编辑 my.ini
将下载的 ZIP 包解压到你要安装 MySQL 的文件夹（如：`d:/dev/mysql/`），然后进入该文件夹，复制 `my-default.ini` 为 `my.ini`，编辑 `my.ini`，在 `[mysqld]` 下添加如下内容：

``` bash
# mysql所在位置，替换为你解压的位置
basedir = d:/dev/mysql
# data文件夹位置，替换为你的解压位置/data
datadir = d:/dev/mysql/data
```

## 2. 初始化 data 文件夹
然后*管理员权限*打开*cmd*进入 MySQL 文件夹下的 bin 文件夹，输入以下内容 [初始化 data 文件夹](https://dev.mysql.com/doc/refman/5.7/en/data-directory-initialization-mysqld.html)：
``` cmd
mysqld --initialize-insecure
mysqld -install
```


## 3. 开启 MySQL 服务
完成以上步骤后，*管理员权限*在 cmd 输入 `net start mysql` 即可开启 MySQL 服务，如下图：
![](https://github.com/csJd/csJd.github.io/raw/res/mysql-zip-install-1.png)

## 4. 加入环境变量，设置 root 密码
为了在命令行直接使用 MySQL，需要把 MySQL 文件夹下的 bin 文件夹加入系统环境变量的 PATH 变量，加入即可。root 默认是没有密码的，初次使用时，在 cmd 输入 `mysql -u root` 即可使用 root 登录 mysql，登入后输入以下命令即可修改 root 密码：
```  mysql
mysql> set password = password('your_new_password");
```
然后就配置完毕了，重装系统后可以直接在 MySQL 文件夹再次配置，不需要再安装安装包。
