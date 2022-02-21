---
title: 智能家居折腾之旅——在32位机器上安装HomeAssistant Supervised
date: 2022-2-21 11:40:33+0800
tags:
- Linux
- Play
categories:
- Play
---

# 智能家居折腾之旅——在32位机器上安装HomeAssistant Supervised
纯新手，前几天买了个小机器打算跑HomeAssistant，没仔细看买了个D2550回来，查询英特尔官方说是64位，可到手装HASS OS才发现装不上，一查资料才明白D2550是i686架构，不完整支持64位，也就是说这玩意实际上是个32位的机器。且以其孱弱的性能无法跑虚拟机，只能想其它办法了。

32位和64位的最大区别就是32位不被Docker官方支持，也就是说常规的安装方法无法适用于32位的X86机器。

<!--more-->
## 准备

首先需要安装最新的Debian系统，必须得是Debian因为HomeAssistant Supervised只支持Debian，而且Debian是唯二给i386机器提供Docker支持的发行版。截至目前（2022-02），Debian的最新版是bullseye（11）。注意一点就是如果在安装Debian时提示需要非自由固件才能继续安装，那么就需要安装带非自由固件版的Debian。

一些前期的必要设置就不细说了，比如配置良好的网络环境。我家因为勉强可以访问到Github就没有进行过多的配置，而且我发现即使用iptables也不能把Docker的流量路由到指定的地方，目前来看可能只有旁路由/主路由这一种方式才能把流量路由到该到的地方。

## 安装
### Docker(-ce)
配置好系统后，就可以开始安装了，首先要安装Docker。
`sudo apt install docker docker.io docker-compose`

然后创建一个软连接，有可能Supervised使用的命令是`docker-ce`。
`ln -s /usr/bin/docker-ce /usr/bin/docker`

### OS-Agent

接着按照官方指导安装`os-agent`：
到[https://github.com/home-assistant/os-agent/releases/latest](https://github.com/home-assistant/os-agent/releases/latest)找到最新的`.deb`结尾的安装包，这里我们选后缀是`.i386.deb`的包。右键复制链接，随后`wget [你复制的链接]`、
`sudo dpkg -i os-agent_1.0.0_linux_x86_64.deb`。

### Supervised

最后是`homeassistant-supervised`，这个在安装的时候，就会创建HomeAssistant的Docker容器并启动，所以确保你的网络连接正常之后再进行安装，否则需要卸载重装。

同样是去[官方的地址](https://github.com/home-assistant/supervised-installer/releases/)复制链接并使用`wget`下载。

然后就是忽略一下依赖：
```shell
sudo dpkg --ignore-depends docker-ce -i homeassistant-supervised.deb
# 然后修改一下依赖
# 在下面这个文件里面找到homeassistant相关的，把depends里面的docker-ce改成docker（我已经改完了）。
sudo vim /var/lib/dpkg/status
# 最后修复一下
sudo apt --fix-broken install
```
如果在安装Supervised的时候出现问题需要重装，那么只需要按照正常软件包的卸载步骤卸载，重复上述安装步骤安装即可。

## 题外话

我本人的步骤其实没有这么简单。参考官方的Requirments之后，我发现其需要`docker-ce`，也就是`Docker Community Edition`，官方还特意强调了这一点。但是从源安装的只有无印版`docker`。所以我一开始的思路是这东西可能需要我编译一个出来。查询发现`docker-ce`在20.10之后分出了两个仓库`docker-cli`和`moby`，其中`moby`就是原来的`docker-engine`。卧槽，说好的社区版呢？后来我想，Github上开源的应该就算社区版，并且我已经有了`docker-cli`，那么直接编译一个`moby`就可以得到`docker-ce`这个可执行文件（`docker`官方也说了正常安装的情况下会得到`docker-ce`，但是我安装的没有并且软件包说需要，这也是我被坑的根本原因之一）。然后我就搜各种编译方法，发现几个坑：

- 编译Docker首先需要Docker
- 虽然Debian提供了Docker，也可以运行不少镜像，但是最为基础的Dockerfile官方不提供i386的版本
- 官方没有裸机编译指导

网上的大部分教程，要不是过时（20.10之后不存在`docker-ce`这个东西），要不就是有了Docker还要搓个Docker出来，只能自己摸索。

实际上裸机编译，看项目根目录下的`Dockerfile`文件就可以知道了，该文件实际上就是先准备一个编译环境的容器出来，然后再对项目进行编译，最后把可执行文件复制进去即可。所以根据官方给的`Dockerfile`就可以进行编译了。Docker是Go语言写的，依赖底层的东西也不是特别多，所以搓一个出来完全可能。

### 但是我为什么没有搓一个出来呢？

因为我仔细看了看那些教程，发现搓出来的可执行文件里面也没有`docker-ce`，可能Docker官方不想区分这个概念了吧，就像Java一样，命令文件名、使用方式相同，保证了对社区版的兼容，但是对于企业用户有定制，这样可以方便企业用户用社区镜像。
