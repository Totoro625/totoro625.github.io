---
title: 在 raspberry 快速恢复生产力环境
date: 2017-01-27 23:24:10
tags: raspberry
category: bash
---

大家都知道我是有多么的健忘（傻）
摧毁了数据库（还不会修复）
来来来 重装一下 raspberry

事后证明是树莓派供电不足，缺少足够的电力导致系统崩溃
<!-- more -->
先在 [官网下载镜像](https://www.raspberrypi.org/downloads/)  并解压，

再搜索 Win32DiskImager 并打开，添加镜像选择 U 盘 write

准备屏幕供电以及键盘鼠标避免停电

`sudo raspi-config`

## 打开配置文件进行设置

1、修改 pi 密码

2、登录选项：auto 登入 GUI 并等待联网

3、语言设置：zh-CN GB2312,zh-CN GB18030,zh-CN UTF-8 空格选中，回车确认，并选择 zh-CN UTF-8 为默认

4、 Advanced Options  高级设置 中开启 Overscan ssh vnc 并选择 Update 更新工具

开启 root 权限

`sudo nano /etc/ssh/sshd_config`

Ctrl + W 快捷键 搜索 PermitRootLogin without-password

修改 PermitRootLogin without-password 为 PermitRootLogin yes

Ctrl + O 快捷键 保存

Ctrl + x 快捷键 退出 Nano 编辑器

重启一下

```
pi@raspberrypi:~$ sudo passwd root
Enter new UNIX password:   #输入第一遍密码
Retype new UNIX password:  #输入第二遍密码
pi@raspberrypi:~$ sudo passwd --unlock root
```

出现提示：密码过期信息已经更新

就可以在电脑端用 root 账户进行管理了（方便复制粘贴代码）

拔掉屏幕键盘鼠标回到电脑上

打开 [oneinstack lnmp 一键安装包](https://oneinstack.com/install/)  对照命令安装。下面是我的生产力（测试）环境安装

```
apt-get -y install wget screen curl python && wget http://mirrors.linuxeye.com/oneinstack-full.tar.gz && tar xzf oneinstack-full.tar.gz && cd oneinstack && screen -S oneinstack
./install.sh

```

(现在觉得lnmp一键包还不错)

//快速进入 lnmp（lnmp lamp lnmpa lnmt lnmh）安装环境

等待这首歌放完（~~可能是我的供电不足导致速度奇慢~~）当然我家电信小水管也是一部分原因

选择默认 ssh 端口`22`；`y `安装 `web server`；默认 `Nginx `默认不安装` Apache`；默认不安装 `Tomcat`；`y `安装数据库；选择 `MySQL5.5 `因为内存小啊，输入数据库密码；默认二进制安装数据库（因为小鸡吖）；

`Y `安装 `php `选择 `php-7 `版本安装；安装默认模式缓存；等等

等待一个小时之后安装完成 `reboot`

再进行附加组件的安装（需要 ssl 证书的话）

`cd oneinstack && ./addons.sh`

（我已经有了通配符证书）

再添加网站

`cd oneinstack &&./vhost.sh`