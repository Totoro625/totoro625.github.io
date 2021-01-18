---
title: Linux搭建Minecraft服务器自用免忘教程【带崩溃自动重启】
date: 2016-12-23 21:57:10
tags: minecraft
category: bash
---
很久之前写的崩溃自动重启源码因为网站数据丢失找不到了，索性从头开始，写一篇完善的教程。

首先要说的是，我一般用的都是centos6.5 64位或者Debian 8.2 64位。均可使用以下教程，但不排除特殊情况，有奇奇怪怪的错误可以联系我。

其次，不喜欢Windows的主机环境，个人认为服务器就应该用shell，弄个图形界面太占用有限的资源，更主要的是贵、、、

首先,你需要一台服务器或者个人计算机运行nix系统(比如:Debian,Ubuntu,RHEL,CentOS,Gentoo,ArchLinux及其衍生版nix
其次,网速很重要.10M光纤大约可以带动30~50人(有数据表明,客户端平均加载的Chunks为12,20M的对等宽带可以带动100~120人,但是已经有部分玩家出现卡顿...其他的自己算把(所谓100M独享,真实下载速度为100M/8=12.5M/s,上下不对等的上传速度为12.5M/8=1.5625M/s,对等的上传速度就有12.5M/s)
最后,内存才是真正的吃,在Linux下2G大约可以带动10~30个人的插件服，5~10个人的小型mod服，2~3人的大型mod服，当你的地图日益庞大之后，请准备3G 4G 6G 8G甚至12G 16G 32G的大型独服。
<!-- more -->
# 第一部分、java的安装

## 1、检测是否安装java

当然、新拿到的vps几乎没有预装好环境的，除了个别奇葩供应商

输入
`java -version`
![](https://totoro.ink/img/TQhn.png)
![](https://totoro.ink/img/ToY8.png)

如果出现
`bash: /usr/bin/java: No such file or directory`
或者
`-bash: java: command not found`
说明本机没有安装java

如果出现
```
java version "1.8.0_18"
OpenJDK Runtime Environment (IcedTea6 1.8.13) (6b18-1.8.13-0+squeeze2)
OpenJDK Client VM (build 14.0-b16, mixed mode, sharing)
```
或者
```
java version "1.8.0_65"
Java(TM) SE Runtime Environment (build 1.8.0_65-b17)
Java HotSpot(TM) Client VM (build 25.65-b01, mixed mode)
```
说明预装了java

## 2、安装java

强烈建议安装Java SE而不是OpenJDK

当然针对懒人和小白可以先用着凑合

### //OpenJDK安装

`yum -y install java-1.8.0-openjdk` //centos
`sudo apt-get install openjdk-6-jre` //debian

### //OpenJDK卸载

`sudo apt-get autoremove openjdk-*`  //debian
`yum -y remove java-* `//centos
查看自己系统版本
`getconf LONG_BIT`
显示64既64位，显示32既32位

### //Java SE的安装

&nbsp;

下载JDK，地址：[http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)，我下载的JDK为8,如图所示：

![](https://totoro.ink/img/TDdW.png)

我们获得了jdk-8u91-linux-x64.tar.gz，使用ftp工具上传。

//以下是Debian安装方式，当然centos也可以用，但不建议

a.通过上面准备工作之后，我们现在已经拥有了可以安装JDK的环境。
b.然后在Xshell中使用命令跳转到local下面创建者自己的文件夹：例如kencery
` cd usr/local/   ` 

`mkdir kencery   `

`cd kencery/`
c.然后使用Xftp将jdk复制到kencery文件夹下面，如图所示：![](https://totoro.ink/img/T2KZ.png)将上传的jdk解压，解压之后重命名为javajdk，如图所示：
 `tar -zxv -f  jdk-8u65-linux-i586.gz`
 `mv jdk1.8.0_65  javajdk`
`cd javajdk`![](https://totoro.ink/img/TBx6.png)
e.通过上面的步骤，我们的jdk已经全部完成安装了，接下来就是更重要的一步：配置环境变量


### 配置环境变量

`vim /etc/profile`
打开之后按键盘（i）进入编辑模式,将下面的内容复制到底部

```
JAVA_HOME=/usr/local/kencery/javajdk
PATH=JAVA_HOME/bin:PATH
CLASSPATH=JAVA_HOME/jre/lib/ext:JAVA_HOME/lib/tools.jar
export PATH JAVA_HOME CLASSPATH
```

备注:根据上面的配置信息，我们既可以将环境变量的配置完成，需要注意的是，PATH在配置的哦时候，一定要把AVA_HOME/bin放在最前面，不然使用java命令式，系统会找到以前的JAVA，在不往下找了，这样java这个可执行文件运行的目录其实不在$JAVA_HOME/bin下，而在其它目录下，会造成很大的问题。
写完之后我们按键盘（ESC）按钮退出，然后按（:wq）保存并且关闭Vim。


配置完成之后，最重要的一步就是使文件立即生效：命令如下：
`source /etc/profile`
**将系统默认的jdk修改过来**

```
$ sudo update-alternatives --install /usr/bin/java java /usr/lib/kencery/jdk1.7.0_05/bin/java 300
$ sudo update-alternatives --install /usr/bin/javac javac /usr/lib/kencery/jdk1.7.0_05/bin/javac 300
$ sudo update-alternatives --install /usr/bin/javaws javaws /usr/lib/kencery/jdk1.7.0_05/bin/javaws 300
$ sudo update-alternatives --config java
$ sudo update-alternatives --config javac
$ sudo update-alternatives --config javaws
```

### 验证是否安装成功

`java -version`
出现
```
java version "1.8.0_65"
Java(TM) SE Runtime Environment (build 1.8.0_65-b17)
Java HotSpot(TM) Client VM (build 25.65-b01, mixed mode)</pre>
<pre class="lang:default decode:true ">echo $JAVA_HOME
```
![](https://totoro.ink/img/TI4x.png)

同样可以
`sudo apt-get autoremove jdk*  //debian`
进行卸载

//如果是centos建议用RPM格式进行安装

简单、简单、简单、简单、简单、重要的事情说五遍

同样在[http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下载

选择JDK*.rpm![](https://totoro.ink/img/TZ04.png)

上传至网站，
`rpm -ivh jdk-*.rpm`
进行安装

一样用
`java -version`
检查即可

可以用
`yum -y uninstall xxx`
进行卸载

# 第二部分、游戏服务端的安装

## 1、喜欢新版本，使用spigot端（支持插件、不支持mod）

访问[downloads/spigot](https://yivesmirror.com/downloads/spigot)选择喜欢的版本进行下载![](https://totoro.ink/img/TKRB.png)

点击红色的部分会有5秒的广告，点击灰色的部分没有广告直接下载

如图所示为最新版本spigot支持最新版本的Minecraft客户端登录

请选择20mb左右的版本进行下载

## 2、选择mod服务器thermos

## （支持mod与插件，最新版本支持1.7.10、也是最广为使用的版本）

现阶段thermos最为简单易用、访问[downloads/thermos](https://yivesmirror.com/downloads/thermos)进行下载

![](https://totoro.ink/img/Tegw.png)

建议选择Thermos-1.7.10-1614-58.zip 这是最新版本且支持1.7.10最新forge版本1614，可以获得最大性能与最大兼容性，当然在一些特殊情况下选择1558或者1449也是可行的

下载完成后将其解压即可

(thermos端运行后会自动下载一个minecraft_server.1.7.10.jar这是正常的)

## 3、初步运行服务端

将你选择的服务端上传至服务器【不建议本地配置文件上传，Win环境测试兼容性可以，不建议作为conf文件修改环境】

建立一个单独的目录，在其.jar目录下新建一个Bash文件或者直接手动执行
`java -jar -Xms512m -Xmx1024m spigot*.jar //512与1024分别为最小最大内存，spigot*部分请写完整`
首次运行可能产生eula.txt文件，主要看你运行的什么服务端

这会使你的服务器开启失败

修改其中的
`eula=false`
为
`eula=true`
即可。

## 4、逐步拖入插件与mod文件

如果没有经过单机测试请一个一个的拖入并开服测试兼容性，开启失败说明不兼容

## 5、使用screen守护mc进程

```
Debian/Ubuntu:
apt-get install screen
CentOS/RHEL
yum install screen
```
之后使用
`screen -S "name"`
其中name可以任你定,不过尽量使用字母,数字组合
然后在里面开服即可(前面有讲解)

## 6、支持盗版登录

别废话，国内不作特别说明都是盗版的
```
nano server.properties //nano修改方式
vi server.properties //vi修改方式
vim server.properties //vim修改方式
```
//或者ftp下载好用notepad修改，切记不要用Windows记事本`
找到
`online-mode=true`
改为
`online-mode=false`
即可使用盗版登录

# 第三部分、进阶：崩服重启

开服模组插件稳定后可以使用崩服重启命令了

你需要一个start.sh 能正常稳定的开启服务器

如前文提到的
`java -jar -Xms512m -Xmx1024m spigot*.jar//512与1024分别为最小最大内存，spigot*部分请写完整`

## 1、长期不维护的自动重启

在服务端目录建立一个`auto.sh`

写上
```
#!/bin/bash
while true 
do
sh start.sh
done
```
`chmod +x auto.sh`
添加可执行权限即可

## 2、长期使用的可维护自动重启

上一个脚本最大弊端在于关不掉

我们于是用这个`autostart.sh`
```
#!bin/sh
while((`cat auto`==1))
do
sh start.sh
done
```
再在当前目录下新建一个名为auto的文件，写入
`1`
`chmod +x autostart.sh`
添加可执行权限

于是乎你每次想让他暂停自动重启的时候修改auto这个文件的值即可

## 3、懒得找到这个文件并且修改它

可以的 使用bash就行，新建一个`change.sh`
`perl -p -i -e "s/1/2/g" auto //auto可以写为/xxx/xx/xx/auto`
将auto文件的1改为2，将他保存至一个sh 文件里面就OK啦

## 4、懒得输入screen【终极方案】【我在用的方案】

可以的，你和我一样懒

解决办法

mc服务器目录写auto文件，内容为【2】【注意是2不是1】
`2`
写一个start.sh，内容为
`java -d64 -server  -Xms1G -Xmx6G  -Dfile.encoding=UTF-8 -jar Thermos.jar
//建议服务端名字统一改写为简单的名字，额外写一个文件记录版本号
//最小内存1G 最大内存6G，针对实际情况填写`
写一个autostart.sh，内容为
```
#!bin/sh
while((`cat auto`==1))
do
sh start.sh
done
```
写一个launch.sh，内容为
```
#!/bin/bash
function start_server {

screen -S mc ./autostart.sh
}

start_server
```
写一个`open.sh`，内容为
```
#!/bin/bash
perl -p -i -e "s/2/1/g" auto
sh launch.sh
```
写一个`close.sh`，内容为
```
#!/bin/bash
perl -p -i -e "s/1/2/g" auto
screen -r mc
```
以上都是在mc服务器目录所写的文件

我们再来到/root目录

写一个`b.sh`，用于日常返回控制台，内容为
```
#!/bin/bash
screen -r mc
```
写一个`o.sh`，用于开服，内容为
```
#!/bin/bash
cd /mc所在目录
sh open.sh
```
写一个`c.sh`，用于关服，内容为
```
#!/bin/bash
cd /mc所在目录
sh close.sh
```
当然，以上全部命令都要加上
`chmod +x 名称.sh`
添加可执行权限

以上就是崩服自动重启的全部内容啦



更多精彩内容还等等我研究完一键脚本以及其他各种脚本之后才会放出补充教程

提前挖坑：

1、有可能搞定的用类似于./mc o   ./mc b    ./mc c等命令进行控制

2、微小可能搞定的用类似于mc o   mc b  mc c等命令进行控制

3、几乎不可能搞定的用mc  然后选择1、2、3、4这样的命令进行控制

请持续关注我的[博客](https://totoro.pub)

# 第四部分、自用简略开服包

## 1、兼容Win与Linux环境最简略版本

第一部分：Windows环境
Windows双击launch.bat即可运行
Windows环境可以删除全部`*.sh`文件以及root目录
第二部分：Linux环境
首先给予全部的可执行权限
` chmod -R 777 /*/*/MC目录`
再将root目录的文件移动到/root
参照懒人办法使用
找不到链接的话直接的访问[我的博客](https://totoro.pub/)
使用方式：
1、长期使用
在root目录（即ssh登录目录）执行`sh o.sh `开服
关闭窗口也不会被结束进程，同时开启崩服自动重启
在root目录（即ssh登录目录）执行`sh b.sh `返回之前的控制台
即可管理服务器，进行各种操作
在root目录（即ssh登录目录）执行`sh c.sh `关服
进行服务器例行维护等管理
2、用于测试环境
`cd /*/*/mc目录`
`sh start.sh`
此时为手动模式，关闭ssh窗口即结束mc进程
注明：游戏中op使用/stop命令即可关服，再配合自动重启功能实现免登陆日常重启


