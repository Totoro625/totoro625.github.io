---
title: 利用Socat实现单端口中继（中转）加速
date: 2016-12-19 19:37:10
tags: network
category: bash
---
iptables端口转发经常会出现一些问题，导致无法使用，而haproxy又不支持转发udp。所以如果你要转发一个或几个端口的话就推荐这个工具：**socat** 。

socat不支持端口段转发，只适用于**单端口或者少量端口**，如果需要大量端口请看下面这两个教程。

*   [利用HaProxy实现中继（中转）加速](https://www.dou-bi.co/ss-jc29/)
*   [利用iptables实现中继（中转）加速](https://www.dou-bi.co/ss-jc34/)
<!-- more -->

## Socat安装

**Centos 系统：**
`yum install -y socat`
**Debian/Ubuntu 系统：**
`apt-get update`
`apt-get install -y socat`

## Socat使用

### 转发TCP

`nohup socat TCP4-LISTEN:2333,reuseaddr,fork TCP4:233.233.233.233:6666  >> /root/socat.log `

***nohup***

指的是 后台运行。

#### TCP4-LISTEN:2333

指的是 监听**ipv4的端口**，也就是 **转发的端口**，后面Shadowsocks链接中继时填写的 **端口**。

#### `fork TCP4:233.233.233.233:6666`

指的是 **被转发的 IP 和 端口**，也就是你要中继的服务器的 **IP** 和 **端口**。
`注意：这里的 **中继端口(2333)** 和 **被中继端口(6666)** 是可以一样的，我区分开只是为了让你们更好地理解。`

#### /root/socat.log 2&gt;&amp;1 &amp;

指的是 **转发日志记录**。

### 转发UDP

`nohup socat UDP4-LISTEN:2333,reuseaddr,fork UDP4:233.233.233.233:6666 &gt;&gt; /root/socat.log 2&gt;`
转发UDP很简单，只要把` TCP4 `改成` UDP4 `就行了！

### 停止转发

`ps -ef | grep socat`
#输入上面的命令找到socat程序的PID，然后用下面的命令KILL掉这个PID进程（PID是个数字，自己替换下面的"pid"）。
`kill -9 pid`

## Socat卸载

Centos系统：
`yum remove socat`
Debian/Ubuntu系统：
`sudo apt-get remove socat`
`sudo apt-get autoremove`

### 简单解释

注意：假设你的**中继服务器**也就是现在在操作的服务器 IP 是` 1.1.1.1 `，那么你的 **中继端口** 就是` 2333 `。你的 **被中继服务器**的 IP 是 `233.233.233.233 `，端口是` 6666 `。

这时候你的 **Shadowsocks客户端** 填写信息的时候 **IP **就是` 1.1.1.1 `，**端口** 就是` 2333 `。

**所以原理就是：**

Shadowsocks客户端通过` 1.1.1.1:2333 `链接**中继服务器**` 1.1.1.1 `，然后**中继服务器**把端口` 2333 `的流量转发到 **被中继服务器**` 233.233.233.233 `的端口` 6666 `上面。然后 **被中继服务器** 也就是上面的 **Shadowsocks服务端**，就会去访问你要的数据，然后依次返回 **中继服务器 -&gt; Shadowsocks客户端**。

## 防火墙设置

如果你设置后无法链接，那么多半是防火墙 阻拦了，只要开放端口 就行了。以上面的 示例的中继端口` 2333 `为例。

```
 iptables -I INPUT -p tcp -m tcp --dport 你的端口 -j ACCEPT  //单端口
 iptables -I INPUT -p tcp -m tcp --dport 12450: -j ACCEPT  //多端口
```



# 注意是半角冒号，意为允许 12450 及以上的端口
# 也可以指定 12450:15550 这样的范围
`iptables -I INPUT -p tcp --dport 2333 -j ACCEPT`
`iptables -I INPUT -p udp --dport 2333 -j ACCEPT`
`service iptables save`
# 保存防火墙
`service iptables restart`
# 重启防火墙
如果上面的命令提示没有这个 服务，那么加上sudo(` sudo service iptables save `/`sudo service iptables restart `)试试或者下面这个。
`/etc/init.d/iptables save`
`/etc/init.d/iptables restart`

## 开机启动

因为这个工具并没有开机启动的设定，所以需要设置系统的开机启动。

**Centos系统：**
`chmod +x /etc/rc.d/rc.local`
`vi /etc/rc.d/rc.local`
**Ubuntu/Debian系统：**
`chmod +x /etc/rc.local`
`vi /etc/rc.local`
输入` I 键 `进入编辑模式（如果没反应请看上面的教程安装 vim），然后在打开的文件中的` exit 0 `代码前面**插入你的 socat 命令代码（就是上面 nohup socat…的代码）。**

然后再 按` ESC 键 `退出编辑模式，然后输入` :wq `退出并保存。
**转载注明：** [Shadowsocks利用 Socat 实现单端口 中继（中转）加速](https://www.dou-bi.co/ss-jc40/)
