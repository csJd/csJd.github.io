---
title: 我的 Gradle 使用实践（零）
tags:
  - Gradle
categories:
  - Practice
date: 2017-03-11 23:14:38
---
Gradle 是一款非常强大的构建工具，最近学习了下 Gradle 的基础使用，这里记录下我的使用实践。
<!-- more -->

## 一. 安装 Gradle

[安装 Gradle](https://gradle.org/install#manually) 直接去官网下载 [最新 zip 包](https://gradle.org/releases)，解压到某个文件夹，然后添加环境变量`GRADLE_HOME`，内容为你解压文文件夹的路径，此目录下含有 `bin` 文件夹。然后再将 `%GRADLE_HOME%\bin` 添加到 PATH 变量中，cmd 执行 `gradle -v`，类似下图则安装成功：
![](http://upload-images.jianshu.io/upload_images/1281889-dcb4090d1cc75b47.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 二. 构建简单的 Java 项目
安装成功后在工作目录创建 `gradle-java` 目录，在此目录下使用 Gradle 进行简单 Java 项目的实践。
### 1. 初始化项目框架
执行 `gradle tasks` 可以查看当前目录的 Gradle 项目可用的所有 Gradle 任务：

![](http://upload-images.jianshu.io/upload_images/1281889-40b52c339ca38589.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`init` 任务可以理解为创建 Gradle 工程初始骨架，可以输入 `gradle help --task init` 查看帮助：
![](http://upload-images.jianshu.io/upload_images/1281889-264ba01db2bc954b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我们执行以下命令就是初始化一个 Java 项目:
``` gradle
gradle init --type java-application
```
执行完毕后目录结构如下：
```plain
gradle-java
│  build.gradle
│  gradlew
│  gradlew.bat
│  settings.gradle
│
├─gradle
│  └─wrapper
│          gradle-wrapper.jar
│          gradle-wrapper.properties
│
└─src
    ├─main
    │  └─java
    │          App.java
    │
    └─test
        └─java
                AppTest.java
```
Gradle 完成了项目基本骨架的创建。

### 2. 运行项目

App.java 文件内容如下：
``` java
/*
 * This Java source file was generated by the Gradle 'init' task.
 */
public class App {
    public String getGreeting() {
        return "Hello world.";
    }

    public static void main(String[] args) {
        System.out.println(new App().getGreeting());
    }
}
```
此时执行 `gradle tasks` 看看这个项目 Gradle 能执行哪些任务：

![](http://upload-images.jianshu.io/upload_images/1281889-020874c8c72f932d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到多了很多新任务，后面再讨论为什么，执行 `gradle run` 看看：

![](http://upload-images.jianshu.io/upload_images/1281889-cdecacfce14764a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

没错，`run` 任务完成了编译运行工作。
