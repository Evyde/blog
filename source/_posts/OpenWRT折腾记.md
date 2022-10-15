---
title: OpenWRT折腾记
date: 2021-03-24 12:56:47  
tags:  
- OpenWRT  
- Play
- Compile
- Linux
categories:  
- Z-Turn  
  
---

有一哥们再也无法忍受小米路由青春版（79那个可以用充电宝供电的巴掌大的路由器）的孱弱性能了，于是考虑购入星际宝盒（矿渣路由器）。由于买来需要用`Dr.COM`，于是问卖家能不能用，卖家说不行。我说：“这`OpenWRT`都装上了，只要有`GCC`就能编译一个`Dogcom`出来，怎么就不行呢？您随便买，买来我帮您装（其实是自己想玩（bushi））。”  
于是就有了这篇文章。
<!--more--> 
  
# 前言
路由器还没拿到手，我先让那哥们装一下`Dogcom`作者编译出来的`ipk`插件试一下，结果发现不行，然后网上也没找到这个CPU的编译成品，我心想坏了，该不会是什么特殊CPU吧，别装逼失败了，赶紧上网查了一下，哎，舒了一口气。该路由的主要配置如下：  
  
|硬件|参数|
|:---:|:---:|
|CPU|高通 ~~骁龙~~ IPQ4019|
|RAM|512M|
|ROM|128M|
|接口|USB3 * 1 + WAN(1Gbps) * 1 + LAN(1Gbps) * 2|
  
  
一查`OpenWRT`官方，还好，这种类型的CPU在官方的支持列表中，不需要自己写`DTS`文件，相当省事。那其实需要做的仅仅就是编译一个插件出来，不需要编译内核和固件。[上网一查](https://jarviswwong.com/compile-ipk-separately-with-openwrt.html)，还真有教程，很新，这意味着不需要自己摸索了（实际上我只看到教程下载`LEDE`然后就自己摸索着做了，浪费了大把时间）。
  
# 编译插件
> 注意！所有操作请在 **非** ROOT环境下进行  
  
按照教程，需要先准备一个虚拟机运行`Ubuntu`系统，这里我选择最新的`Ubuntu 20.04 LTS`版本。然后准备`OpenWRT`官方的`SDK`，在我这里，我通过查看路由器的`OpenWRT`版本发现是大于19的，那我就下载最新的`19.07.5`版本的SDK，找到`ipq40XX`然后找到[openwrt-sdk-19.07.5-ipq40xx-generic_gcc-7.5.0_musl_eabi.Linux-x86_64.tar.xz](https://archive.openwrt.org/releases/19.07.5/targets/ipq40xx/generic/openwrt-sdk-19.07.5-ipq40xx-generic_gcc-7.5.0_musl_eabi.Linux-x86_64.tar.xz)文件下载下来解压为`openwrt-sdk`备用。克隆`LEDE`到本地，将`lede/package`下的文件全部拷贝并覆盖至`openwrt-sdk/package`。  

更新依赖：
```shell
sudo apt-get update
sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3.5 unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib linux-libc-dev:i386 ccache
```

安装软件包：
```shell
cd openwrt-sdk
./scripts/feeds update -a && ./scripts/feeds install -a
```

接下来下载我们的主角，[Dogcom](https://github.com/mchome/openwrt-dogcom)，`Dogcom`分为两个部分，适用于`Luci`的图形化界面部分和主程序部分，所以需要将两个部分都克隆下来。
```shell
cd package
git clone https://github.com/mchome/openwrt-dogcom
git clone https://github.com/mchome/luci-app-dogcom
```
  
> 当然，您也可以通过修改`luci-app-dogcom`中的文件来自定义界面风格

使用
```shell
make menuconfig
```

来自定义编译的软件包，找到`Network`下的`dogcom`，按`M`，意思是单独编译出插件。同理请在`Luci`下找到`luci-app-dogcom`，同样选择`M`。退出时选择`OK`保存。  
查看`Makefile`文件发现，我们克隆下来的项目实际上只有Makefile，需要编译的文件则要另外下载。所以请使用代理运行：
```shell
make download -j8 V=s
```

我使用了`proxychains-ng`来进行命令行的代理，具体操作是：
```shell
proxychains4 make download -j8 V=s
```

等待下载完成。

按照这个教程来说，下面就该编译插件了，但是我之前是参考的`LEDE`项目的`README`文件，所以直接执行了`make -j1 V=s`等了半个多小时（第一次执行为了少出错使用了`-j1`单线程编译参数）。

> **注意：请您千万不要模仿我**

然后，请使用以下命令来编译`Dogcom`，应该会很快。
```shell
make package/openwrt-dogcom/compile -j1 V=s
make package/luci-app-dogcom/compile -j1 V=s
```

编译完成后请在`bin/packages/你的路由器架构/base/`目录下找到插件。

我建议，您可以直接创建一个本地HTTP服务器来进行插件安装，这样就不必上传到路由器了。
使用（Port为端口号，默认为8000）
```shell
python3 -m http.server <port>
```

来创建一个临时的HTTP服务器，在路由器中执行`opkg install http://192.128.192.128:8000/xxx.ipk`来进行安装。

# 安装其它插件
这里以`OpenClash`为例，来介绍如何安装其它插件。  
首先找到`OpenClash`的官方介绍，发现其给出一个所有版本的插件的下载地址（如果没有给出，可以参考上述教程编译），下载后备用。再看其介绍中提到一些依赖包，可以进行安装，但是安装前建议先检查软件源可用性，您可以参考[清华源](https://mirrors.tuna.tsinghua.edu.cn/help/openwrt/)进行一键更改，也可以像我一样闲地蛋疼手动替换。
  
> **注意：不要复制网上的源，复制前请改成与自己CPU架构相符的源地址！**

由于我这个商家魔改版`OpenWRT`已经自定义了许多模块，我这里就只查看缺少的模块并安装。使用
```shell
opkg udpate
opkg install xxxx
```

来进行插件的安装。  
有一个问题，就是自己编译的内核容易存在版本号不符的情况，比如我这个，版本号显示`5.4.105`，但是官网的版本号后面带有MD5，类似`5.4.105-1-0799b5c93f2634f64ed642d02c3e5221`，这里请酌情操作，我的建议是选择版本号相同的最老的官方版本的插件进行安装。比如`5.4.105`这个版本，官网有两个版本号：
```
5.4.105-1-0799b5c93f2634f64ed642d02c3e5221
5.4.105-1-5aa3a3e37e4bdda90d7ea8c9467221f2
```

我就选择上面的`0799b5c`来进行下载。并在`opkg`命令中加入`--force-depends`来进行强制安装，如：
```shell
opkg install kmod-ipt-extra.ipk --force-depends
```

这样就会忽略内核版本号来进行安装。
  
> 注意：这样做很可能会导致不稳定的错误，请谨慎操作

然后就可以正常安装了，但是安装完成后发现无法启动，提示缺`libcap`及`libcap-bin`，但是我似乎已经安装，后来一看版本号发现是太老了，这里需要注意一下，从官网下载`0.43`版本的重新安装即可。
  
  
# 题外话  
其实我敲`make -j1 V=s`的时候就意识到不对劲了，编译整个项目才需要这样，后来一查才发现，我误打误撞地摸索出了`SDK`没出来之前操作办法，就是先编译一套工具链（toolchain）出来，然后再进行编译，这样的好处是可以编译一些`OpenWRT`官方没有，但是常规`Linux`发行版里有的东西。比如我之前一看到`OpenClash`缺`curl`，第一反应不是配置软件源进行一键安装，而是直接源码编译安装，有当年初中手搓LAMP那味儿了。  
  
  
### 以下是在已经有完整交叉编译工具链的情况下的杂谈，本人不保证以下方法能用，仅供学习使用
前面说到，我不小心误打误撞编译了整个固件，也就顺便编译了`zlib`和`openssl`，所以我先下载了[curl](https://curl.se)，解压并且编译安装，命令如下：
```shell
wget https://curl.se/download/curl-7.75.0.tar.gz
tar zxvf curl-7.75.0.tar.gz
mv curl-7.75.0 curl
cd curl
mkdir openwrt-build
./configure --prefix=/home/evyde/curl/openwrt-build --host=arm-openwrt-linux \
--with-libssl-prefix=/home/evyde/lede/build_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/openssl-1.1.1j/ipkg-install/usr/lib \
CC=/home/evyde/lede/staging_dir/toolchain-arm_cortex-a7+neon-vfpv4_gcc-8.4.0_musl_eabi/bin/arm-openwrt-linux-gcc \
CXX=/home/evyde/lede/staging_dir/toolchain-arm_cortex-a7+neon-vfpv4_gcc-8.4.0_musl_eabi/bin/arm-openwrt-linux-g++ \
--with-zlib-prefix=/home/evyde/lede/build_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/zlib-1.2.11/ipkg-install/usr/lib \
--with-zlib \
--with-mbedtls-prefix=/home/evyde/lede/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib \
--with-ca-bundle=/etc/ssl/certs/ca-certificates.crt
make
make install
```

如果您之前编译完工具链后重启过，还需要设置一下`STAGIN_DIR`等环境变量。  
虽然我也没编译出来，提示`mbedtls`有问题，但是依照我查到的资料来看，这样应该是可以正常编译的，由于我没有这个需求，这篇文章就到此为止了。

> 再次提醒：请您不要模仿我
  
