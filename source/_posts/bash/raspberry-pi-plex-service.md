---
title: raspberry 搭建plex服务器
date: 2018-01-31 00:21:10
tags: raspberry
category: bash
thumbnail: https://totoro.ink/img/oqfS.png
---

raspberry安装plex的速度实在是太慢了，网上教程很少，错误很多，故写一篇教程。

由于树莓派是armhf架构，而plex官方并没有提供相应的软件包，我又不会自己编译，只能上网找教程/编译好的包

我在网上东找西找，最后找到一篇这样的文章-[使用树莓派搭建私人媒体云](https://www.zybuluo.com/jocker---/note/840749#5-plex-media-server) 最后根据自己的经验成功的在raspberry上安装了plex
<!--more-->
他的文章中[添加源的方法](https://www.zybuluo.com/jocker---/note/840749#5-plex-media-server)已经不再可用，不过我根据他提供的源地址，顺藤摸瓜找到了作者的博文-[Plex Media Server for the Banana Pi and Raspberry Pi 2](https://dev2day.de/post/plex-media-server/) ，并在[packages](https://dev2day.de/pms/dists/jessie/main/binary-armhf/Packages)中找到了deb包[所在的路径](https://dev2day.de/pms/pool/main/p/)

作者给出了4种包：

- [plexmediaserver-installer/](https://dev2day.de/pms/pool/main/p/plexmediaserver-installer/)
- [plexmediaserver-neonless-installer/](https://dev2day.de/pms/pool/main/p/plexmediaserver-neonless-installer/)
- [plexmediaserver-neonless/](https://dev2day.de/pms/pool/main/p/plexmediaserver-neonless/)
- [plexmediaserver/](https://dev2day.de/pms/pool/main/p/plexmediaserver/)

我依据更新日期选择了最新的plexmediaserver-installer 更新日期是2017-12-14 01:29 ，还算挺新的

```
wget https://dev2day.de/pms/pool/main/p/plexmediaserver-installer/plexmediaserver-installer_1.10.1.4602-f54242b6b-1_armhf.deb
dpkg -i plexmediaserver-installer_1.10.1.4602-f54242b6b-1_armhf.deb
service plexmediaserver restart
```

正常安装完后`/var/lib/plexmediaserver`目录权限是正确的，如有错误可以

`chown -R plex:plex /var/lib/plexmediaserver` 来更正错误

之后就可以正常访问`http://192.168.x.xx:32400/web/` 来访问web控制端了

![控制端](https://totoro.ink/img/ojdn.png)

如果还是不会使用plex的话可以看一下[这篇文章](https://yorkchou.com/plex-video-stream.html ) 
