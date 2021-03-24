---
title: raspberry 使用心得记录
date: 2018-01-16 23:50:10
tags: raspberry
category: bash
---

## 系统的选择

*绝大部分错误/故障都可以通过重装系统解决*

先在 [官网下载镜像](https://www.raspberrypi.org/downloads/) 并解压，

再用Win32DiskImager写入 U 盘

在windows下唯一能显示的boot分区下建立新的文件SSH（注意无后缀） 以开启ssh链接功能
<!-- more -->
用户：`pi` 

密码：`raspberry`

## 开启 root 权限

*更加方便的使用*

`sudo nano /etc/ssh/sshd_config`

Ctrl + W 快捷键 搜索 PermitRootLogin without-password

修改 PermitRootLogin without-password 为 PermitRootLogin yes

Ctrl + O 快捷键 保存

Ctrl + x 快捷键 退出 Nano 编辑器

```
pi@raspberrypi:~$ sudo passwd root
Enter new UNIX password:   #输入第一遍密码
Retype new UNIX password:  #输入第二遍密码
pi@raspberrypi:~$ sudo passwd --unlock root
```

出现提示：密码过期信息已经更新

重启一下

就可以在电脑端用 root 账户进行管理了

## 使用WiFi连接

*摆脱网线的困扰*

`nano /etc/wpa_supplicant/wpa_supplicant.conf`

在尾部添加

```
network={
   ssid=""
   psk=""
}
```

## 更换国内源

*加快软件安装速度。实测stretch源是必须的，否则总是会在raspberry.org下载软件包*

`nano /etc/apt/sources.list`

进入编辑界面，删除原有的内容，添加下方的源内容

```
deb http://mirrors.aliyun.com/raspbian/raspbian/ wheezy main non-free contrib
deb-src http://mirrors.aliyun.com/raspbian/raspbian/ wheezy main non-free contrib
deb http://mirrors.aliyun.com/raspbian/raspbian/ stretch main non-free contrib
deb-src http://mirrors.aliyun.com/raspbian/raspbian/ stretch main non-free contrib
```

或者中科大

```
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ wheezy main non-free contrib
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ wheezy main non-free contrib
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main non-free contrib
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main non-free contrib
```

或者清华大学

```
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ wheezy main non-free contrib
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ wheezy main non-free contrib
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main non-free contrib
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main non-free contrib
```

使用`apt-get update`更新软件列表

再用`apt-get upgrade` 更新软件包

## 一些软件

"树霉派"能感受啥呢？这里有一些应用推荐给大家

### LNMP

*网页显示的基础*

`apt-get install screen git vim htop`

`screen -S lnmp`

`wget -c http://soft.vpser.net/lnmp/lnmp1.4.tar.gz && tar zxf lnmp1.4.tar.gz && cd lnmp1.4 && ./install.sh lnmp`

详见[LNMP一键包](https://lnmp.org/install.html)

树莓派上的版本不用追的太高，部分新版本软件可能都没适配，基本默认即可。不推荐MySQL，如果服务器上有尽量使用远程数据库，降低负荷。

### Syncthing

*简单的介绍一下就是个同步网盘，可以多终端同步，支持版本控制*

之前有给大家推荐过[resilio sync](https://totoro.pub/bash/raspberry-resilio-sync.html) 可惜鉴于互联网形式的严峻，几乎不能够使用了，有兴趣的还可以参照那篇文章操作一下

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

### caddy的File Manager

*差不多算是私人网盘了，包括linux简单命令执行，具备上传下载分享功能*

*caddy本身相当于一份模块化Nginx，注意不要冲突*

File Manager是caddy中的一个插件，但是其本身也是非常强大

我们在下载页面选择Linux ARMv7 ，插件选上 http.filemanager ，版权选择个人用户即可下载了

例如：我下载到的是caddy_v0.10.10_linux_arm7_custom_personal.tar.gz，将其上传到树莓派并解压

并给予可执行权限

建议丢在root目录，因为破程序的执行不是很完善

在其所在目录新建一个Caddyfile文件，这样可以./caddy直接启动带配置的程序，否则需要使用./caddy -conf ../path/to/Caddyfile

为了方便使用，你还可以吧caddy文件复制一份到/usr/bin(或者建立软链接)，这样可以使用caddy 代替./caddy命令

值得注意的是执行命令的目录下有Caddyfile文件就可以直接调用配置文件，否则需要caddy -conf ../path/to/Caddyfile

这样一来，丢root目录的优势就显现出来了。

Caddyfile内可写

```
:2018 {
root /home
gzip
timeouts none
filemanager / /home {
  database /home/filemanager.db
  }
}
```

这样，访问ip:2018即可查看File Manager

默认用户:密码是admin:admin

你会发现它的强大之处的，相信我。

#### 优化

由于caddy启动之后会有log，除了screen手动启动之外

我们还可以用nohup来进行无log启动

`nohup ./caddy >>/root/log_caddy.txt 2>&1 &`

用`ps`来查找进程，用`kill`来消灭进程

上述代码可以加入开机启动文件内，实现开机启动

### h5ai

*网站文件目录访问*

通过他可以做到简单美观的网站资源在线浏览

从[官网](https://larsjung.de/h5ai/)下载到[源码](https://release.larsjung.de/h5ai/h5ai-0.29.0.zip)后上传并解压即可食用

caddy可以通过简单的配置实现下载文件的功能，但是依旧略有欠缺

```
:2019 {
root /home
gzip
browse
}
```

如果使用了nginx则需要添加一段代码开启文件目录访问

```
		location ~/ {
                autoindex on;
                autoindex_exact_size off;
                autoindex_localtime on;
                    }
```

访问/_h5ai/public/index.php可以检查还有那些地方可以优化

一般我会去除一些被禁用的 PHP 函数：

`nano /usr/local/php/etc/php.ini`
搜索 `scandir`、`exec`、`passthru`，将其从被禁用的函数中删除。

[更多优化教程可查看](https://www.htcp.net/3643.html)

我就不过多介绍了

### chevereto图床

[checereto图床](https://chevereto.com/)是一个相对知名的图床解决方案，分[付费](https://chevereto.com/pricing)和[免费](https://github.com/Chevereto/Chevereto-Free)两种方案（付费版经常节假日期间对折出售），正常我们食用[免费版](https://github.com/Chevereto/Chevereto-Free/releases/latest)即可满足需求

如果使用了nginx则需要添加一段代码开启程序的正常访问

```
		location / {
if (-f $request_filename/index.html){
rewrite (.*) $1/index.html break;
}
if (-f $request_filename/index.php){
rewrite (.*) $1/index.php;
}
if (!-f $request_filename){
rewrite (.*) /index.php;
}
try_files $uri $uri/ /api.php;
}
location /admin {
try_files $uri /admin/index.php?$args;
}
```

### 更多

待续ing