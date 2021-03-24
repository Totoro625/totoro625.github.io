---
title: systemctl的使用
date: 2018-01-23 17:03:00
update: 2018-01-23 17:03:00
tags: systemctl
category: bash
---

*用上了caddy然而每次都要手动启动，不方便也不优雅*

*于是学习了systemctl的使用方法*

Caddy提供了一个官方的systemd文件
<!--more-->
```
curl -s https://raw.githubusercontent.com/mholt/caddy/master/dist/init/linux-systemd/caddy.service -o /etc/systemd/system/caddy.service
```

然后启用他，并开启开机自启

```
systemctl daemon-reload
systemctl enable caddy.service
systemctl start caddy.service
```

然而在我的使用过程中却不断报错

将其简化后反而可以使用了

```
[Unit]
Description=Caddy HTTP/2 web server
After=network-online.target
Wants=network-online.target systemd-networkd-wait-online.service

[Service]
Restart=on-abnormal
ExecStart=/data/caddy/caddy -log stdout -agree=true -conf=/data/caddy/Caddyfile -root=/var/tmp
KillMode=mixed
KillSignal=SIGQUIT
TimeoutStopSec=5s
LimitNOFILE=1048576
LimitNPROC=512

[Install]
WantedBy=multi-user.target
```

也是很迷

