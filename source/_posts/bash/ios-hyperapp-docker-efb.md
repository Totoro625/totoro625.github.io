---
title: IOS 服务器管理软件 HyperApp 推荐，Linux 上自动化部署 Docker，附上 EFB 教程
date: 2017-06-20 07:30:10
tags: docker
category: bash
---
# 一、前言

> 对于从来没接触过linux的人来说，HyperApp绝对不是你选择尝试Linux的理由，请相信我。不然你可能连服务器都登不上
> 上面是一个FLAG，不管你信不信，反正我是信的

对于一个新手小白来说，Linux的困难甚至是ssh登录层面的

<!-- more -->

在这里我的建议是找一家可以按量付费的IDC，先开一台，认真读读几篇说明文档，弄懂如何使用ssh，还有进阶的ssh key的使用。这里我推荐的是[国内的腾讯云](https://www.qcloud.com/)，原因是阿里云的按量付费要预充值￥100+

腾讯云对于学生用户更加友好，虽然服务不及阿里云，但是感觉态度上是友好的（[学生优惠政策](https://www.qcloud.com/act/campus)，1元学生机，可选HK）

能够一键生成SSH KEY这一点感觉挺好用，不放心的可以下载puttygen.exe（[下载地址](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)）自行生成密钥对

![enter description here][1]

这是我的KEY ，选择一个点击进去就可以看到公钥（也是腾讯云带我走进密钥登录这个坑的，这里表扬一下）

公钥差不多ssh-rsa AAAXXXX........这种形式，记住这个应该不会用错了吧（当然是我曾经搞错过）

多用ssh登录几次服务器，再换成密钥登陆几次差不多就粗略熟悉了Linux的最最最基本的登录操作了，就相当于你会给Windows开机了

&nbsp;

# 二、HyperAPP

本文是介绍HyperAPP这个软件的，在APP STORE搜索HyperAPP，中国售价￥25，[美区$3.99](https://itunes.apple.com/us/app/hyperapp/id1179750280)

![enter description here][2]

一杯咖啡不是么？毕竟是独立开发首个docker管理APP，界面挺简洁，支持不支持？（不支持我也没办法：淘宝美区兑换码，封号几率30%）

唯一比较遗憾的是并不支持openvz架构（因为依赖docker）



![enter description here][3]

这是我在用的界面


![enter description here][4]


当然是可以简单管理SS的，适合少量端口的私人机场

为什么我要推荐这个APP呢？

原因是<del>他的widget简直太好看了</del>

![enter description here][5]

不，可以使用EFB

# 三、EFB食用说明

> EH Forwarder Bot（简称 EFB）是一个可扩展的聊天平台隧道框架，基于 Python 3。目前已内置了 Telegram 主端 (Master Channel) 和微信从端 (Slave Channel)，用来在 Telegram 收发微信消息。
> 也就是说，有了EFB我们现在就可以使用telegram来收发WeChat的全部消息啦

这种好事一定要支持

奈何目前EFB的教程非常少，我作为一个资深蠢货手动安装总是失败，偶然拾得HyperApp的一键docker简直不要太爽（懒人专属）

## 3.1创建 Telegram Bot

使用 [@BotBrother](https://telegram.me/botfather) 创建一个新的机器人（点击[链接](https://telegram.me/botfather)即可，不要像我一样蠢到在搜索栏里面搜索）

发送指令 `/newbot`，根据提示完成创建，之后得到一个 `token`

（此处图片丢失）

接下来还要对刚刚启用的 Bot 进行进一步的配置：允许 Bot 读取非指令信息、允许将 Bot 添加进群组、以及提供指令列表：

*   发送 `/setprivacy` 到 @BotFather，选择刚刚创建好的 Bot 用户名，然后选择 Disable；
*   发送 `/setjoingroups` 到 @BotFather，选择刚刚创建好的 Bot 用户名，然后选择 Enable；
*   发送 `/setcommands` 到 @BotFather，选择刚刚创建好的 Bot 用户名，然后发送如下内容：

    link - 将会话绑定到 Telegram 群组
    chat - 生成会话头
    recog - 回复语音消息以进行识别
    extra - 获取更多功能    `</pre>
    这里先告一段落，我们然后还需要获取你自己的 Telegram ID，向 [@get_id_bot](https://t.me/get_id_bot) （点这个[链接](https://t.me/get_id_bot)）发起会话便可。

    Your Chat ID :后面的一串数字就是你的ID啦

    此时我们拥有两个东西

1.  机器人的token

2.  自己的chat id
    接下来我们配置服务器上的EFB

    ## 3.2服务端EFB配置（HyperApp）

    在HyperApp的docker商店找到EFB并安装，配置文件填入之前获得的token及chat id等待安装好即可使用

   （此处图片丢失）

3.  安装完毕后点击“查看日志” -&gt; 点击右上角的 "Term" 在终端中打开，然后点击右上角的"A-" 缩小字体直到能看见二维码。

4.  使用微信扫描该二维码。注意不能截图保存到相册中然后选择图片扫描，只能扫描屏幕。但你可以截图后发到另外一台设备（比如通过QQ/微信发到PC版上）然后用微信扫描。

    ## 3.3服务端EFB配置（手动）

    [教程1](https://alanoy.com/post/send-and-receive-wechat-message-with-telegram/)    [教程2](https://blog.1a23.com/2017/01/09/EFB-How-to-Send-and-Receive-Messages-from-WeChat-on-Telegram-zh-CN/)

    大致上是

    ```
    git clone https://github.com/blueset/ehForwarderBot.git
    cd ehForwarderBot
    pip3 install -r requirements.txt
    mkdir storage chmod 777 storage
    cp config.sample.py config.py
    editor config.py
    ```

手动出问题的都是python3的安装上出现的问题（没错我就是）

更多docker应用教程见[GITHUB](https://github.com/waylybaye/HyperApp-Guide)

## 3.4 EFB的使用

刚刚得到EFB的我也是不太会用

对你的机器人说

“/chat ”可以选择一个新的对话

“ /chat 乌龟” 就可以查找通讯录里名字带乌龟的人

你可以通过长按新会话选择回复来跟那个人聊天

人太多会显得混乱？

在电脑上新建一个群组把你和你的机器人拉进去

然后对你的机器人说“/link”选择联系人或者群组“/link 乌龟”选择带乌龟的联系人或者群组到一个群，这样你直接在哪个群组里面说话而不需要长按回复了，不建议在群组里拉入第三人除非你要被人查岗

“/group”可以将公众号拉入群组

这样你可以给他们设置不同的通知提醒

建议吧公众号拉倒一个群组，因为你也不需要给公众号发消息


[1]: https://totoro.ink/img/hyperapp01.png
[2]: https://totoro.ink/img/hyperapp02.jpg
[3]: https://totoro.ink/img/hyperapp03.jpg
[4]: https://totoro.ink/img/hyperapp04.png
[5]: https://totoro.ink/img/hyperapp05.jpg