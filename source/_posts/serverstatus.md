---
title: ServerStatus支持多台服务器监控的探针
date: 2018-01-23 17:18:00
update: 2018-01-23 17:18:00
tags: 
 - status
 - 服务器
category: bash
---

*服务器多了就总是想着监控----这大概是病，得治*

ServerStatus是一款python探针，[英文原版项目](https://github.com/BotoX/ServerStatus) | [中文版项目](https://github.com/cppla/ServerStatus)

另外也有Go 语言编写的[stathub-go](https://github.com/likexian/stathub-go/blob/master/README-ZH.md) 可供选择
<!--more-->
以下内容转载自[桜の流年](https://min.kiwi/serverstatus/)

ServerStatus是一个用Python开发的探针的多主机探针，可以简单实现负载、网络、流量、CPU、内存和磁盘等参数的监控，如图

![ServerStatus](https://totoro.ink/img/serverstatus.png)

在使用探针之前我们需要克隆探针代码，执行以下命令

```
git clone https://github.com/cppla/ServerStatus
```

可以看到克隆的ServerStatus里面分为三个文件夹，其中Server为服务器端主程序，Client为客户端探针，而Web文件夹中则是探针的页面部分

## 服务端配置

首先编译服务器端程序

```
cd ServerStatus/server
make
```

运行刚刚编译好的程序，如果没有错误，就可以按Ctrl+C退出，接着下一步了

```
./sergate

[server]: Bound to :35601   这是正常状态的显示
[server]: Couldn't open socket. Port (35601) might already be in use.   这种状态是35601端口被占用
```

然后需要修改config.json文件，注意username, password的值要与客户端保持一致

修改完成之后，可以把编译完成的程序和配置文件放到/usr/local/bin和/etc下，以免堆在root目录下不便于管理

```
cp -r ./sergate /usr/local/bin/
cp -r ./config.json /etc/sergate.json
```

下面到探针页面派上用场的时候了，回到根目录下，复制ServerStatus/web到你的网站路径

```
cp -r ServerStatus/web/* /data/wwwroot/status
```

一切配置完成之后，可以启动服务端了，注意

```
sergate --config=/etc/sergate.json --web-dir=/data/wwwroot/status
```

## 客户端配置

客户端有两个版本，client-linux为普通的linux版本，client-psutil为跨平台版本

如果普通版配置不成功，可以换成跨平台的版本试试，首先进入客户端的目录

```
cd ServerStatus/client
```

客户端的配置很简单，只需要修改相应的参数，首先vim client-linux.py修改SERVER地址，username帐号， password密码，然后python client-linux.py运行无错误就可以了。

跨平台版本也一样，但是要先安装psutil跨平台依赖库，然后和普通版一样vim client-psutil.py修改SERVER地址，username帐号， password密码，最后python client-psutil.py运行无错误就大功告成。

为了方便管理，可以把client-linux.py或client-psutil.py也复制到/usr/local/bin或者/opt下

## psutil库安装方法

CentOS Python2

```
yum -y install epel-release
yum -y install gcc python-devel python-pip
pip install psutil
```

CentOS Python3

```
yum -y install epel-release
yum -y install gcc python34 python34-setuptools
easy_install-3.4 pip
pip3 install psutil
```

Debian/Ubuntu Python2

```
apt-get -y install python-setuptools python-dev build-essential
apt-get -y install python-pip
pip install psutil
```

Debian/Ubuntu Python3

```
apt-get -y install python3-setuptools python3-dev build-essential
apt-get -y install python3-pip
pip3 install psutil
```

## 设置Systemd服务

刚才的服务端程序和客户端探针都是手动启动的，但是对于正式使用的场合肯定不能这样子手动启动

可能有些人会想到把它直接丢进rc.local，我不建议这样做，不可控，不易管理

那么怎么样自启动才是最好的呢，当然是创建系统服务了，拜systemd所赐，现在不再需要写天书般的启动脚本了

服务端脚本示例     /etc/systemd/system/serverstatus-server.service

```
[Unit]
Description=ServerStatus Server Daemon
Wants=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/sergate --config=/etc/sergate.json --web-dir=/data/wwwroot/status

[Install]
WantedBy=multi-user.target
```

执行以下命令安装并启动

```
systemctl daemon-reload
systemctl enable serverstatus-server.service
systemctl start serverstatus-server.service
```

客户端脚本示例     /etc/systemd/system/serverstatus-client.service

```
[Unit]
Description=ServerStatus Client Prober
Wants=network.target

[Service]
Type=simple
ExecStart=/usr/bin/python3 /opt/client-linux.py

[Install]
WantedBy=multi-user.target
```

执行以下脚本安装并启动

```
systemctl daemon-reload
systemctl enable serverstatus-client.service
systemctl start serverstatus-client.service
```