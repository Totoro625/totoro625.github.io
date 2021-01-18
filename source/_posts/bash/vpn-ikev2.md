---
title: VPN服务器一键搭建教程(ipsec,ikev2)
date: 2017-06-02 00:56:10
tags: 
  - network
  - vpn
category: bash
---
# 安装部分

使用 [一键包](https://github.com/quericy/one-key-ikev2-vpn)

先下载

```
wget --no-check-certificate https://raw.githubusercontent.com/quericy/one-key-ikev2-vpn/master/one-key-ikev2.sh
chmod +x one-key-ikev2.sh
```

准备好 SSL 证书（单域名的，那么多免费的，随意撸一个一年的呗）
<!-- more -->
在脚本所在的目录新建 3 个文件

1. **ca.cert.pem** 证书颁发机构的 CA；
2. **server.cert.pem** 签发的域名证书；（注意检查两个证书不要颠倒）
3. **server.pem** 签发域名证书时用的私钥；

决定以后都不改 ip 之后就把 SNAT 规则打开

看到 install Complete 字样即表示安装完成。默认用户名密码将以黄字显示，可根据提示自行修改配置文件中的用户名密码, 多用户则在配置文件中按格式一行一个 (多用户时用户名不能使用%any), 保存并重启服务生效。

最终跑完之后 连接信息是：

服务器地址：域名 ip

账户：myUserName

密码：myUserPass

这么愚蠢的默认连接方式怎么能用？

`vi /usr/local/etc/ipsec.secrets`

修改 myUserName %any : EAP “myUserPass” 为对应的用户名与密码

可以添加多用户（封锁这么严重了，顶多给你家妹子用就行了）

把： /usr/local/sbin/ipsec 加入到：/etc/rc.local 来实现需要开机自启

不怕麻烦的可以（当然有些奇怪的问题的时候也可以用）

```
ipsec start   #启动服务
ipsec stop    #关闭服务
ipsec restart #重启服务
ipsec reload  #重新读取
ipsec status  #查看状态
ipsec --help  #查看帮助
```

或者用 /etc/init.d/ipsec_vpn start|stop|restart 来彰显高端

更多问题请去 [gtihub](https://github.com/quericy/one-key-ikev2-vpn/issues)

# IOS 配置

找到设置=》VPN=》添加配置，

默认类型都是 IKEv2 了吧，描述随便写，服务器与远程 ID 填写上面说到的 域名 ，用户鉴定为 用户名 ，分别填写你 设置的用户名与密码或者默认的 myUserName   myUserPass

然后就可以愉快的玩耍啦

打游戏什么的还是 VPN 靠谱