---
title: 树莓派上安装 resilio-sync
date: 2017-6-28 16:05:10
tags: raspberry
category: bash
---

​	这篇教程参考了 Resilio 官网教程 [Installing Sync Package On Linux](https://help.getsync.com/hc/en-us/articles/206178924-Installing-Sync-package-on-Linux) ，并针对[树莓派](http://nodis.cn/tag/%e6%a0%91%e8%8e%93%e6%b4%be/)进行了优化，使用套件库安装的优点是自动配置好 Sync 相关服务，无需手动配置。

## 准备工作：

1. [树莓派](http://nodis.cn/tag/%e6%a0%91%e8%8e%93%e6%b4%be/)3b一台（其他版本类似），系统为 Raspbian；
2. 挂载好硬盘，因为同步或下载需要空间，TF 卡显然放不下；
3. 树莓派连接到局域网，并可以通过 SSH 访问。

## 安装 resilio-sync

根据官方教程，在树莓派上面安装 resilio-sync 套件，只需三步：

1. 添加库；
2. 添加用于套件验证的PGP公钥；
3. 安装套件。
<!-- more -->
由于树莓派的 Raspbian 系统基于 Debian ，所以我们要按照下面的教程安装：

### 添加库

创建文件 `/etc/apt/sources.list.d/resilio-sync.list` 并写入下面的内容以注册 Resilio 套件库：

`deb http://linux-packages.resilio.com/resilio-sync/deb resilio-sync non-free`

### 添加公钥

使用下面的命令添加公钥：

`wget -qO - https://linux-packages.resilio.com/resilio-sync/key.asc | sudo apt-key add -`

对于树莓派2和3（ arm64 架构） 还要运行下面的命令

`sudo dpkg --add-architecture armhf `

`sudo apt-get update`

然后将 `/etc/apt/sources.list` 中的内容修改为

`deb [arch=armhf] http://linux-packages.resilio.com/resilio-sync/deb resilio-sync non-free`

对于树莓派1则运行下面的命令

`sudo dpkg –-add-architecture armel`

### 安装 resilio-sync 套件

使用下面的命令

`sudo apt-get update `

`sudo apt-get install resilio-sync`

对于树莓派1则运行

`sudo apt-get update `

`sudo apt-get install resilio-sync:armel`

安装完成后，使用下面的命令删除旧版 btsync（可选）

`sudo apt-get purge btsync`

## 使用

`rslsync --webui.listen 0.0.0.0:8888`

使用 IP 加端口号即可进入管理页面，默认端口号为 `8888`，例如访问 `127.0.0.1:8888`，第一次使用需要创建用户名和密码（请务必牢记），其他设置和桌面版类似，包括免费使用PRO的方法。

## 备注

使用上面的方法安装完 Sync 之后，再次运行 `sudo apt-get update` 会提示下面的警告信息：

> W: Duplicate sources.list entry [http://linux-packages.resilio.com/resilio-sync/deb/](http://linux-packages.resilio.com/resilio-sync/deb/) resilio-sync/non-free armhf Packages (/var/lib/apt/lists/linux-packages.resilio.com_resilio-sync_deb_dists_resilio-sync_non-free_binary-armhf_Packages)
>
> W: You may want to run apt-get update to correct these problems

解决方法是删除第一步里面创建的这个这个文件 `/etc/apt/sources.list.d/resilio-sync.list`。

