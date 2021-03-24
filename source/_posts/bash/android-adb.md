---
title: adb命令模拟按键事件 KeyCode
date: 2018-02-13 02:18:00
update: 2018-02-13 02:18:00
tags: android
category: bash
---

随手重启手机之后，由于输入法未自启动无法输入锁屏密码

半夜两点多爬起来想办法

依据[这篇文章](http://blog.csdn.net/skycnlr/article/details/68922205)成功输入了密码` adb shell input text "hello,world"`

再依据[这篇文章](http://blog.csdn.net/jlminghui/article/details/39268419)成功输入了回车进入了系统`adb shell input keyevent 66`

还好我够机灵

![adb](https://totoro.ink/img/adb.png)

不多说了，回头再补充

先睡了

等等，改成数字密码先。。。