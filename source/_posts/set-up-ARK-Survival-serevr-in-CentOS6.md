---
title: 在CentOS 6上搭建 方舟：生存进化 服务器
date: 2018-02-17 00:14:13
tags:
- ARK
- Install
- Linux
- Server
- Game
categories: Game
---
# Foreword
  In 2018, Steam had been started a Chinese Spring Festival Selling Activity, I buy a ARK: Survival Evolved in this activity. Though I can't play it at the moment, but I can setup a server. Luckily, I have an avaliable VPS.

<!--more-->
# Contents
  > - [Prepare](#Prepare)
  > - [Installation](#Installation)
  >   - [Update glibc](#Update-glibc)
  >   - [Update gcc](#Update-gcc)
  >   - [Install Third Party Dependency](#Install-Third-Party-Dependency)
  >   - [Install arkserver](#Install-arkserver)
  > - [Usage](#Usage)
  >   - [All Commands](#All-Commands)
  >   - [Running](#Running)
  >   - [Updating](#Updating)
  >   - [Debugging](#Debugging)
  >   - [Configure LinuxGSM](#Configure-LinuxGSM)
  >   - [Further Information](#Further-Information)

# Prepare
  > - A 4GB RAM or higher VPS
  > - CentOS 6 or higher system

# Installation
## Update glibc
  In CentOS 6, we must update our glibc from 2.14 to 2.16. First, switch to root user or you should add `sudo` before every command.
  ```bash
  su
  ```
  Then, input these commands to have prerequisites.
  ```bash
  yum groupinstall "Development tools"
  yum install glibc-devel.i686 glibc-i686
  ```
  Change directory to /tmp
  ```bash
  cd /tmp
  ```
  Download the version 2.16.0 of glibc
  ```bash
  wget http://ftp.gnu.org/gnu/glibc/glibc-2.16.0.tar.gz
  ```
  Unarchive it
  ```bash
  tar zxvf glibc-2.16.0.tar.gz
  ```
  Change directory to glibc-2.16.0
  ```bash
  cd glibc-2.16.0
  ```
  Make a new directory to save build files
  ```bash
  mkdir glibc-build
  ```
  Change to it
  ```bash
  cd glibc-build
  ```
  Run configure
  ```bash
  ../configure --prefix='/usr'
  ```
  > Notice: There are two `.` before the word `configure`.

　

  > Tips: `--prefix='XXX'` means the directory where the glibc will install.

  Then,we need to have some changes in the file `test-installation.pl`
  ```bash
  vim ../scripts/test-installation.pl
  ```
  To jump to 171th line, press `:` and type `171` and `ENTER`.
  And replace `if (/$ld_so_name/) {` with `if (/\Q$ld_so_name\E/) {`
  Then install it
  ```bash
  make && make install
  ```

## Update gcc
  > Notice: This operation takes a lot of space! Be sure to have enough!

  Like the last step, so I gave commands only.
  ```bash
  cd /tmp
  wget ftp://ftp.gwdg.de/pub/misc/gcc/releases/gcc-4.6.4/gcc-4.6.4.tar.gz
  tar -xvzf gcc-4.6.4.tar.gz
  cd gcc-4.6.4
  ./contrib/download_prerequisites
  mkdir objdir
  cd objdir
  ../configure --prefix=/opt/gcc-4.6.4
  make
  make install
  mv /usr/lib64/libstdc++.so.6 /usr/lib64/libstdc++.so.6.bak
  mv /opt/gcc-4.6.4/lib64/libstdc++.so.6 /usr/lib64/libstdc++.so.6
  mv /opt/gcc-4.6.4/lib64/libstdc++.so.6.0.16 /usr/lib64/libstdc++.so.6.0.16
  ```

## Install Third Party Dependency
  In 64-bit system
  ```bash
  yum install mailx postfix curl wget bzip2 gzip unzip python binutils bc tmux glibc.i686 libstdc++ libstdc++.i686
  ```
  In 32-bit system
  ```bash
  yum install mailx postfix curl wget bzip2 gzip unzip python binutils bc tmux libstdc++
  ```
  > Notice: If there's not tmux, change your repo by inputing
  > ```bash
  rpm -Uvh http://mirror.webtatic.com/yum/el6/latest.rpm
  yum install epel-release
  ```
  > or input
  > ```bash
  wget http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
  rpm -ivh epel-release-6-8.noarch.rpm
  ```
  > For 32-bit user, replace `x86_64` with `i386`.

## Install arkserver
  Switch to root.
  ```bash
  su
  ```
  Create a user.
  ```bash
  useradd arkserver
  passwd arkserver
  ```
  Find where the sudoers is.
  ```bash
  whereis sudoers
  ```
  Add it to sudoers. And you should replace `/etc/sudoers` with the path your `whereis` command found.
  ```bash
  chmod u+x /etc/sudoers
  vim /etc/sudoers
  ```
  Find the line where you can see `root ALL=(ALL)       ALL`. It is near 90th line.
  Then create a new line after this line, type
  ```bash
  arkserver ALL=(ALL) ALL
  ```
  Switch permission and login
  ```bash
  chmod u-x /etc/sudoers
  su - arkserver
  ```
  Download and run the script
  ```bash
  wget -N --no-check-certificate https://gameservermanagers.com/dl/linuxgsm.sh && chmod +x linuxgsm.sh && bash linuxgsm.sh arkserver
  ```
  Run the installer and follow the instructions
  ```bash
  ./arkserver install
  ```
# Usage
## All Commands
  A complete list of commands can be found by typing
  ```bash
  ./arkserver
  ```
  Below are the most common commands available.
## Running
### Start
  ```bash
  ./arkserver start
  ```
### Stop
  ```bash
  ./arkserver stop
  ```
### Restart
  ```bash
  ./arkserver restart
  ```
### Console
  Console allows you to view the live console of a server as it is running and allow you to enter commands; if supported.
  ```bash
  ./arkserver console
  ```
  > Notice: To exit the console press `CTRL`+`B`+`D`.  
  > Pressing `CTRL`+`C` will terminate the server.


## Updating
### Update
  Update checks for any server updates and applies them. The server will update and restart only if required.
  ```bash
  ./arkserver update
  ```
  Bypass the check and go straight to SteamCMD update.
  ```bash
  ./arkserver force-update
  ```
### Validate
  You can use the SteamCMD validate option when updating the server.
  ```bash
  ./arkserver validate
  ```
## Debugging
### Details
  You can get all important and useful details about the server such as passwords, ports, config files etc.
  ```bash
  ./arkserver details
  ```
### Debug
  Use debug mode to help you if you are having issues with the server. Debug allows you to see the output of the server directly to your terminal allowing you to diagnose any problems the server might be having.
  ```bash
  ./arkserver debug
  ```
### Logs
  Server logs are available to monitor and diagnose your server. Script, console and game server (if available) logs are created for the server.
  ```bash
  /home/arkserver/logs
  ```
### Backup
  Backup will allow you to create a complete bzip2 archive of the whole server.
  ```bash
  ./arkserver backup
  ```
### Monitor
  LinuxGSM can monitor the game server by checking that the proccess is running and querying it. Should the server go offline LinuxGSM can restart the server and send you an alert. You can use cronjobs to setup monitoring.
  ```bash
  ./arkserver monitor
  ```

## Configure LinuxGSM
  For details on how to alter LinuxGSM settings visit [LinuxGSM Config Files page][1].
## Further Information
  For more information and documentation about LinuxGSM see the [wiki][2] that will give you plenty of detail about how everything works.

[1]: https://github.com/GameServerManagers/LinuxGSM/wiki/LinuxGSM-Config-Files
[2]: https://gameservermanagers.com/wiki
