---
title: raspberry 使用说明以及应用推荐
date: 2017-10-26 22:45:10
tags: raspberry
category: bash
---

# raspberry 使用说明

先在 [官网下载镜像](https://www.raspberrypi.org/downloads/) 并解压，

再搜索 [Win32DiskImager（或我提供的下载地址）](https://do.totoro.pub/For_win/win32diskimager-1.0.0-install.exe) 并打开，添加镜像选择 U 盘 write

在windows下唯一能显示的boot分区下建立新的文件SSH（注意无后缀） 以开启ssh链接功能
<!-- more -->
用户：`pi` 

密码：`raspberry`

使用`sudo raspi-config`打开配置文件进行设置

需要修改的有以下几点：

1、修改 pi 密码

2、登录选项：auto 登入并等待联网

3、语言设置：zh-CN GB2312,zh-CN GB18030,zh-CN UTF-8 空格选中，回车确认，并选择 zh-CN UTF-8 为默认

4、 Advanced Options 高级设置 中开启 Overscan ssh vnc 并选择 Update 更新工具

### 开启 root 权限

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

就可以在电脑端用 root 账户进行管理了

### 使用WiFi连接

`nano /etc/wpa_supplicant/wpa_supplicant.conf`

在尾部添加

```
network={
   ssid=""
   psk=""
}
```

### 更换国内源

`nano /etc/apt/sources.list`

进入编辑界面，删除原有的内容或者用#注释掉原来的源，添加下方的源内容

```
deb http://mirrors.aliyun.com/raspbian/raspbian/ wheezy main non-free contrib
deb-src http://mirrors.aliyun.com/raspbian/raspbian/ wheezy main non-free contrib
```

或者中科大

```
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ wheezy main non-free contrib
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ wheezy main non-free contrib
```

或者清华大学

```
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ wheezy main non-free contrib
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ wheezy main non-free contrib
```

使用`apt-get update`更新软件列表

### 一些软件

"树霉派"能感受啥呢？这里有一些应用推荐给大家

之前有给大家推荐过[resilio sync](https://totoro.pub/bash/raspberry-resilio-sync.html) 可惜鉴于互联网形式的严峻，几乎不能够使用了，有兴趣的还可以参照那篇文章操作一下

#### Syncthing

[Syncthing](https://syncthing.net/)是一款新型的同步软件，类似于Resilio Sync，但是它是开源的，更重要的是，他保留了NAT功能，这使得它在封锁严重的国度依旧可以正常工作。

参考[「玩物志」Syncthing的安装与使用](http://www.jianshu.com/p/4235cc85c32d)

我们需要下载[ARM](https://github.com/syncthing/syncthing/releases/download/v0.14.39/syncthing-linux-arm-v0.14.39.tar.gz)的文件来供树莓派使用

```
wget https://github.com/syncthing/syncthing/releases/download/v0.14.39/syncthing-linux-arm-v0.14.39.tar.gz
tar xzvf syncthing-linux-arm-v0.14.39.tar.gz
cd syncthing-linux-arm-v0.14.39
cp syncthing /usr/local/bin
```

之前下载和解压出来的文件可以全部删掉了

```
cd ~
rm -rf syncthing*
```

到这里我的Syncthing已经安装好了，可是直接运行的话，并不能通过外网访问到管理页面，因为Syncthing的管理页面默认是只有本机可以访问的，所以接下来还要进行一点修改，先运行Syncthing

```
screen -S th
syncthing
```

随后就会看到有很多信息，看到类似以下内容的时候就可以按`CTRL-C`退出程序了

```
[OH4IP] 13:32:15 INFO: Completed initial scan (rw) of folder edatb-zzc5f
[OH4IP] 13:32:15 INFO: Device OH4IPQD-QDCDAZB-YMMZE4F-BAK4BLQ-3EZLPTD-V73J37V-LTW44V6-YSM6JQ7 is "ruter.ga" at [dynamic]
[OH4IP] 13:32:15 INFO: Loading HTTPS certificate: open /root/.config/syncthing/https-cert.pem: no such file or directory
[OH4IP] 13:32:15 INFO: Creating new HTTPS certificate
[OH4IP] 13:32:15 INFO: GUI and API listening on 127.0.0.1:8384
[OH4IP] 13:32:15 INFO: Access the GUI via the following URL: http://127.0.0.1:8384/
[OH4IP] 13:32:16 INFO: Detected 0 NAT devices
```

我们第一次运行是为了让它创建配置文件，然后我们再进行修改。用以下命令对配置文件进行编辑:

```
nano ~/.config/syncthing/config.xml
```

一瞬间是不是懵逼了？不要慌，先找到下面这几行:

```
<gui enabled="true" tls="false" debugging="false">
    <address>127.0.0.1:8384</address>
    <apikey>2GeGJK9z6tXKP3nHJYU56ZHoYSYnqQ9S</apikey>
    <theme>default</theme>
</gui>
```

然后把IP`127.0.0.1`修改成`0.0.0.0`即可保存退出:

```
<gui enabled="true" tls="false" debugging="false">
    <address>0.0.0.0:8384</address>
    <apikey>2GeGJK9z6tXKP3nHJYU56ZHoYSYnqQ9S</apikey>
    <theme>default</theme>
</gui>
```

设置好之后执行`syncthing`运行，就可以通过`http://your_ip_addr:8384`来进行访问管理了，如果直接通过外网ip:端口访问还是无法打开管理页面，那就需要进行防火墙的设置开启8384端口了:

```
iptables -I INPUT -p tcp --dport 8384 -j ACCEPT
service iptables save
service iptables restart
syncthing
```

再次打开`http://your_ip_addr:8384`就能看见管理页面了