---
title: hexo安装
date: 2017-06-21 02:38:10
tags: hexo
category: bash
---

# 开始

centos：`yum -y install git gcc gcc-c++ curl wget screen htop`

ubuntu：`apt-get -y install git gcc gcc-c++ curl wget screen htop`

# 安装nodejs

```
git clone https://github.com/cnpm/nvm.git && cd nvm && sh ./install.sh ; source $HOME/nvm/nvm.sh ; source $HOME/.nvm/nvm.sh 
```

```
nvm install v0.10.32 && nvm use 0.10
```
<!-- more -->

# 安装hexo

```
npm install -g hexo
```

使用mkdir创建目录

使用hexo init初始化

示例

```
mkdir /home/wwwroot && hexo init /home/wwwroot
```



# 部署 Hexo --- 配置文件

title站点标题

subtitle站点子标题

description站点描述信息

author站点管理员

language: zh-CN

theme: 选择主题



# lnmp

来源[lnmp.org](https://lnmp.org/install.html)

```screen -S lnmp```

```
wget -c http://soft.vpser.net/lnmp/lnmp1.4.tar.gz && tar zxf lnmp1.4.tar.gz && cd lnmp1.4 && ./install.sh lnmp
```

由于内存较低选择MySQL5.5即可，PHP大可追求最新版







