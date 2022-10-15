---
title: Java - 搭建开发环境
date: 2016-10-30 01:33:25
tags:
- Java
categories:
- Code
---
# 写在前面
要想进行开发Java，就必须要搭建开发环境。现在虽然有些[IDE（点击查看）](https://baike.baidu.com/item/%E9%9B%86%E6%88%90%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/298524?fromtitle=IDE&fromid=8232086&type=syn)可以自动帮助搭建，但是这样你永远不会明白怎么搭建环境，也就是说你对工具的依赖太大。搭建Java开发环境很麻烦，但是学习中必须迈出的一步。
<!--more-->

# 目录
  > - [准备工作](#准备工作)
  > - [下载JDK](#下载JDK)
  > - [安装JDK](#安装JDK)
  > - [设置环境变量](#设置环境变量)
  >   - [Windows 7/8/10系统](#Windows-7-8-10系统)
  >   - [Linux系统](#Linux系统)
  > - [测试安装情况](#测试安装情况)
  >   - [Windows系统](#Windows系统)
  >   - [Linux系统](#Linux系统)
  > - [成功](#成功)

# 准备工作
> * JDK安装包
> * 好的搜索引擎
> * 无比的耐心

# 下载JDK
JDK目前最新的版本是1.8，最好去官网下载，[点击前往](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)。
如图：

> ![JDK下载界面][1]

看到`Java SE Development Kit 8u111`，选择`Accept License Agreement`。接着选下面的，Windows系统32位的选`jdk-8u111-windows-i586.exe`，64的选`jdk-8u111-windows-x64.exe`，其余系统按照提示选择下载即可。
如果搞不清楚该下载哪个版本，可以使用百度搜索[JDK](https://www.baidu.com/s?wd=JDK)，出现一个百度软件中心的结果，点击`普通下载`即可。

# 安装JDK
下载完成后安装，在选择目录的时候记住安装目录，以Windows系统为例，我的是`F:\Program Files\Java\jdk1.8.0_111`。然后一直单击下一步，一般是没有什么问题直接会安装好的。

# 设置环境变量
## Windows 7/8/10系统
右键单击桌面上`此电脑`，选择`属性`，在左面选择高级系统设置，出现如下窗口：

> ![高级系统设置][2]

选择`高级`选项卡，点击`环境变量`，如图：

> ![环境变量][3]

在`系统变量`一栏点击`新建`，在弹出的窗口中对应填入：

> 变量名：JAVA_HOME
> 变量值：F:\Program Files\Java\jdk1.8.0_111

变量名可以随便起，变量值就是刚刚安装JDK的目录。然后选择`Path`变量，单击`编辑`，在`变量值`的最前面加入

> %JAVA_HOME%\bin**;**

注意后面的分号。单击`确定`保存，这时应该是配置成功了。

## Linux系统
用vim打开/etc/profile文件，在文件底部加入如下片段：
```bash
JAVA_HOME=/usr/java/jdk1.8.0_111/
PATH=$JAVA_HOME/bin:$PATH
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME
export PATH
export CLASSPATH
```
根据你的安装目录更改其中的值。保存退出。

# 测试安装情况
## Windows系统
同时按下`Windows键`和`R键`，弹出一个名为`运行`的窗口，在其中输入`cmd`，打开命令行界面，输入`javac -version`，若出现版本号的提示，说明安装成功。若出现`‘javac’不是内部或外部命令，也不是可运行的程序`，回头检查环境变量的设置和JDK的安装。

## Linux系统
```bash
javac -version $$ echo $?
```
如果显示0，说明成功。
# 成功
至此，Java开发环境的搭建算是完成了。如果我们需要开发程序，只需要使用任意的无格式的文本编辑器即可。
