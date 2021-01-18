---
title: 树莓派 Raspberry Pi 启用 root 登陆账户
date: 2016-12-22 18:20:10
tags: raspberry
category: bash
---
树莓派系统使用的linux是debian系统，所以树莓派启用root和debian是相同的。

debian里root账户默认没有密码，但账户锁定。

当需要root权限时，由默认账户经由sudo执行，Raspberry pi 系统中的Raspbian

默认主机名是 raspberrypi 默认用户是 pi 密码为 raspberry

为了方便折腾，建议第一时间启用 ROOT 账号吧~ 这个也很简单的，只需要执行一下两句命令即可：
*   [利用iptables实现中继（中转）加速](https://www.dou-bi.co/ss-jc34/)
<!-- more -->

// 重新开启root账号，可由pi用户登录后，执行此命令后系统会提示输入两遍的root密码，输入你想设的密码即可。

```
pi@raspberrypi:~$ sudo passwd root
Enter new UNIX password:   #输入第一遍密码
Retype new UNIX password:  #输入第二遍密码
```

// 启用 root 账号登录

```
pi@raspberrypi:~$ sudo passwd --unlock root
passwd: password expiry information changed.
```


输入上面第一行代码 第二行是提示错误的代码

原因是 新版本ssh默认关闭root登陆 你可以修改一下ssh的配置文件
`pi@raspberrypi:~$ sudo nano /etc/ssh/sshd_config`
**Ctrl + W 快捷键 搜索 ** **PermitRootLogin without-password**

**修改 PermitRootLogin without-password 为 PermitRootLogin yes**

**Ctrl + O 快捷键 保存**

**Ctrl + x 快捷键 退出 Nano 编辑器**

执行完之后，用 reboot 命令重启，这样就可以解锁root账户