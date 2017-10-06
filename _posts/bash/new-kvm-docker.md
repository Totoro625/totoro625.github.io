---
title: 一个新的服务器到手后(kvm)
date: 2017-07-15 01:58:10
tags: docker
category: bash
---

## 进行环境配置

`yum -y install screen git htop nano vim`



## 更好的上网配置

`wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/shadowsocks_install/master/ssr-install.sh && bash ssr-install.sh` 	来自 [91yun](https://www.91yun.co/archives/14321)

`vi /home/ssr/mudb.json`

修改`chacha20`为`chacha20-ietf` 以更适合移动设备等运算能力低或者有省电需求的设备

```
添加用户： ssr adduser
删除用户： ssr deluser
启动 SSR ： ssr start
停止 SSR ： ssr stop
重启 SSR ： ssr restart
卸载 SSR ： ssr uninstall
更新 SSR ： ssr update
```
<!-- more -->

## 开启BBR

```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
chmod +x bbr.sh
./bbr.sh
```

来自[秋水逸冰](https://teddysun.com/489.html)

### 开启成功验证方法（可省略

重启完成后，进入 VPS，验证一下是否成功安装最新内核并开启 TCP BBR，输入以下命令：

```
uname -r
```

查看内核版本，含有 4.11 就表示 OK 了

```
sysctl net.ipv4.tcp_available_congestion_control
```

返回值一般为：
net.ipv4.tcp_available_congestion_control = bbr cubic reno

```
sysctl net.ipv4.tcp_congestion_control
```

返回值一般为：
net.ipv4.tcp_congestion_control = bbr

```
sysctl net.core.default_qdisc
```

返回值一般为：
net.core.default_qdisc = fq

```
lsmod | grep bbr
```

返回值有 tcp_bbr 模块即说明bbr已启动



## Resilio Sync搭建

新建一个`/etc/yum.repos.d/resilio-sync.repo`

写入

```
[resilio-sync]
name=Resilio Sync $basearch
baseurl=http://linux-packages.resilio.com/resilio-sync/rpm/$basearch
enabled=1
gpgcheck=1
```

再导入密钥`rpm --import https://linux-packages.resilio.com/resilio-sync/key.asc`

`yum -y update`

`yum -y install resilio-sync`

就算安装成功了

`rslsync --webui.listen 0.0.0.0:8888 `	开启网页访问

树莓派参考[这个方法](https://totoro.pub/bash/raspberry-resilio-sync.html)

或者使用

```
wget https://download-cdn.resilio.com/stable/linux-x64/resilio-sync_x64.tar.gz
tar -zxvf  resilio-sync_x64.tar.gz
./rslsync --webui.listen 0.0.0.0:8888
```

```
管理命令：
sudo service resilio-sync start
sudo service resilio-sync stop
sudo service resilio-sync restart
卸载：
sudo yum remove btsync
```

更多请参照[官方说明文档](https://help.resilio.com/hc/en-us/articles/206178924)



### 导入Resilio Sync密钥并等待同步

需要注意的是：

- 网站数据文件同步
- 数据库导出导入
- Nginx文件导出导入
- 旧域名301



## lnmp的安装

`screen -S lnmp`

`wget -c http://soft.vpser.net/lnmp/lnmp1.4.tar.gz && tar zxf lnmp1.4.tar.gz && cd lnmp1.4 && ./install.sh lnmp`

根据需要来就行，建议使用**MySQL 5.5**  我比较喜欢**PHP7**

参考[lnmp一键包](https://lnmp.org/install.html)



##  用daocloud进行docker集群管理

进入 [daocloud](https://www.daocloud.io/) 之后，我们在控制台进行[集群管理](http://dashboard.daocloud.io/cluster) 

不用怕，就算我们只有一台服务器也是可以j进行集群管理的

点击可爱的添加主机键

咦，等一下，我的docker是不是还没有安装？

对于网络好的国外服务器可以使用 `curl -sSL https://get.docker.com/ | sh` 进行安装

对于其他服务器大可使用[DaoCloud提供的镜像](http://get.daocloud.io/#install-docker) `curl -sSL https://get.daocloud.io/docker | sh` 进行安装

当然他们还提供了[各种加速服务](http://get.daocloud.io/)给国内的服务器

之后进行安装主机监控程序

执行DaoCloud给的命令将你的服务器与DaoCloud相连接



## 部署docker

### 简要说明

h5ai是一个精致的文件目录浏览程序，但是配置复杂

owncloud是一个网盘程序，但是在Nginx环境下各种坑

Resilio Sync是一个bt同步程序

但是我为什么不用docker来部署Resilio Sync呢？因为docker部署的Resilio Sync需要额外的权限配置，不利于全局的文件同步



在发现镜像中找到[corfr/h5ai](http://dashboard.daocloud.io/packages/d6c0f231-d83d-44f9-89fa-f1c237fd4dfd) 和[library/owncloud](http://dashboard.daocloud.io/packages/7d1b2f49-c52a-4a8d-b678-b917f6c0ccd2) 即可选择部署

在容器端口选项中将动态端口改为好记的固定端口



### 对于Volumes

h5ai为

| 容器路径     | 主机路径            | 是否可写 |
| -------- | --------------- | ---- |
| /var/www | /home/sync/h5ai | 是    |

这样你在/home/sync/h5ai添加的文件将会直接展示出来

owncloud为

| 容器路径                 | 主机路径                       | 是否可写 |
| -------------------- | -------------------------- | ---- |
| /var/www/html/apps   | /home/sync/owncloud/apps   | 是    |
| /var/www/html/config | /home/sync/owncloud/config | 是    |
| /var/www/html/data   | /home/sync/owncloud/data   | 是    |

这样你的owncloud数据处理数据库都在这里了

进入yourip/phpmyadmin/ 进行数据库管理，导入备份过的数据库再去owncloud配置即可正常使用了



## 开启nginx反向代理实现SSL以及HSTS

`lnmp vhost add` 先添加虚拟主机，按照流程并选择添加Letsencrypt的证书

成功后在`/usr/local/nginx/conf/vhost` 修改配置信息

示例：https://pan.totoro.pub

```
server
    {
        listen 80;
        #listen [::]:80;
        server_name pan.totot.net ;
		return	  301 https://pan.totot.net$request_uri;		//重定向http到https
}

server
    {
        listen 443 ssl http2;
        #listen [::]:443 ssl http2;
        server_name pan.totoro.pub ;
        index index.html index.htm index.php default.html default.htm default.php;
        root  html;			//修改为html
		add_header Strict-Transport-Security "max-age=18036000; includeSubdomains; preload";	//HSTS 180天
        ssl on;
        ssl_certificate /etc/letsencrypt/live/pan.totoro.pub/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/pan.totoro.pub/privkey.pem;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
        ssl_ciphers "EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5";
        ssl_session_cache builtin:1000 shared:SSL:10m;
        # openssl dhparam -out /usr/local/nginx/conf/ssl/dhparam.pem 2048
        ssl_dhparam /usr/local/nginx/conf/ssl/dhparam.pem;

        #error_page   404   /404.html;

		location / {
        proxy_pass http://127.0.0.1:你的端口;  //本地的端口
		}
		
        access_log off;
    }
```

对于3个需要使用端口访问的服务使用反向代理并添加SSL加密使得数据的传输更加安全



## 博客程序的部署

### hexo

我所使用的是hexo是在本地计算机创建的静态页面直接用sync同步过去即可，更多hexo的使用请参照你所使用的主题说明文档或者本博客其他文章

### chevereto图床

参考[官网](https://chevereto.com/) ，我是用的是[chevereto free](https://github.com/Chevereto/Chevereto-Free) 版本

同样这里可以使用[furiousgeorge/chevereto](http://dashboard.daocloud.io/packages/e9b826c3-3184-440c-a3f3-17c80ed3a8e2) docker，但是我的数据积累较多暂时没迁移

主要是懒得看说明不想配置Volumes啦

使用sync同步所有数据之后，导入数据库

Nginx的conf需要增加一段

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

#location / {
#try_files $uri $uri/ /index.php?$query_string;
#}
```

当然使用docker不用这么麻烦，直接使用上文的方法即可



## reboot之后需要做的

`rslsync --webui.listen 0.0.0.0:8888`  开启Resilio Sync

`service docker start` 开启docker使用

**（加入开机启动偶尔会不执行，手动输入一下吧**





