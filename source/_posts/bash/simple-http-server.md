---
title: Simple HTTP Server 快速搭建http web服务+一键脚本
date: 2016-12-19 19:17:10
tags: network
category: bash
---

有的时候你可能需要临时下载VPS中某个文件，但是又没有或者不想用SFTP去下载，那你可能就需要SimpleHTTPServer了。

SimpleHTTPServer是python自带的一个HTTP服务，可以方便的搭建一个临时的HTTP服务，提供文件浏览和下载的web服务。

<!-- more -->

## 使用步骤

注意，因为SimpleHTTPServer是python自带的一个组件，所以使用这个命令的前提就是**系统已安装python**。

如果你只是临时想要下载几个VPS中的文件，那你可以**直接使用**，如果你是需要频繁的开放某个文件夹，以HTTP下载文件，那你适合**使用脚本**。
<pre>注意：如果你开放的文件夹中有` index.html `文件，那会直接显示这个文件，如果没有这个文件那会以 文件列表的形式 显示目录下所有文件。</pre>
无论使用那种方法创建HTTP服务，创建完成之后都可以通过` http://VPS-IP:8000 `访问你刚才开放的文件夹，默认是8000 可以自己改。

### 直接使用

#### 前台运行：

首先` cd `到你要开放的文件夹中，然后使用下面的命令可以把 当前文件夹内的所有文件 发布到VPS的` 8000 `端口。

但是这条命令是直接在前台运行，不是后台运行的，也就是说如果` Ctrl + C `，则该端口就会关闭。
`python -m SimpleHTTPServer 8000`

#### 后台运行：

在上述命令的最后加一个` &amp; `，则该命令产生的进程在后台运行，不会影响当前终端的使用。

生成的新的进程为当前SSH的子进程，所以，当我们关闭当前SSH链接时，相应的子进程也会被kill掉，这也不是我们想要的结果。
`python -m SimpleHTTPServer 8000 &`
在命令的开头加一个` nohup `，忽略所有的挂断信号，如果当前SSH链接关闭，则当前进程会挂载到init进程下，成为其子进程，这样即使退出当前用户，其 8000 端口也可以使用。
`nohup python -m SimpleHTTPServer 8000 &amp;`

#### 结束进程：

如果你是直接用第一个命令前台运行，那你可以直接使用` Ctrl + C `来关闭HTTP服务。

如果你使用` &amp; `或者` nohup `把进程放到了后台运行，那你就需要使用下面这个命令结束进程。
`eval $(ps -ef | grep "[0-9] python -m SimpleHTTPServer" | awk '{print "kill "$2}')`

### 脚本使用

无论使用那种方法创建HTTP服务，创建完成之后都可以通过` http://VPS-IP:8000 `访问你刚才开放的文件夹，默认是8000 可以自己改。

下载启动/停止脚本，并赋予执行权限。
`wget -N --no-check-certificate https://cdn.itoto.cc/sh/pythonhttp.sh &amp;&amp; chmod +x pythonhttp.sh`

#### 启动HTTP服务器：

`bash pythonhttp start`

#### 停止HTTP服务器：

`bash httpstop.sh stop`
查看日志：
`cat httpserver.log`
如果你需要持续的查看/监控日志，那可以使用这个命令：
`tail -f httpserver.log`
启动脚本后，会提醒你输入要开放的HTTP端口 和 目录。

```
请输入要开放的端口 [1-65535]:
(默认端口: 8000):
请输入你要开放的目录(绝对路径):
(直接回车, 默认当前文件夹):
========================
请检查配置是否正确 !
端口 : 8000
```


目录 : /root
========================

按任意键继续，如有错误，请使用 Ctrl + C 退出.

HTTP服务器已经启动。 地址: http://VPS_IP:8000
脚本默认的SimpleHTTPServer端口是` 8000 `，默认回车是开放脚本当前所在的 目录。

如果你需要重新开一个其他目录的HTTP服务器，那么不需要
`bash pythonhttp.sh stop`
停止，可以直接
`bash pythonhttp.sh start`
因为会自动检测并停止已经运行的HTTP服务器。



这个脚本很简单，也只是我随手写的，因为主要考虑到这个命令本身就是为了快捷的开放一个临时的HTTP服务，如果写的太复杂就失去了意义。



**转载注明：**[逗比根据地](https://www.dou-bi.co/) » [『原创』SimpleHTTPServer 快速搭建HTTP Web服务 + 一键脚本](https://www.dou-bi.co/wlzy-8/)
