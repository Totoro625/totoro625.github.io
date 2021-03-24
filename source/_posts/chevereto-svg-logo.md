---
title: chevereto矢量化 LOGO(SVG) 与 WebP 的反优化
date: 2018-01-19 23:49:00
update: 2018-01-19 23:49:00
tags: chevereto
category: bash
---

### 矢量Logo（SVG）

chevereto在仪表盘很贴心的给出了一份启用矢量Logo（SVG）的选项

一直以来我都不会添加矢量化图标，原因是Photoshop生成的SVG格式chevereto不认同，为此我专门下载了AI来生成SVG，然而依旧不行
<!--more-->
在走访了几个chevereto站点 [存图库](https://cuntuku.com/) [PIC HOST ](https://pic.freejishu.com/) [路过图床](https://imgchr.com/)之后，分析了他们的logo，只有前两个用上了SVG

![存图库](https://cuntuku.com/content/images/system/logo_1497449401431_7c0d73.svg) ![PIC HOST](https://totoro.ink/img/pichost.svg) ![TOTORO625图床](https://totoro.ink/img/totoroimg.svg)

查看了他们SVG的代码后，发现只需要在SVG文件的开头添加上

`<?xml version="1.0" encoding="utf-8"?>` 

即可正常被chevereto程序识别为真的SVG

### 又拍云CDN的反优化

上传完LOGO即可看到变换，然而那个丑陋的图案真的是我设计的么？

真的不是我上传错了?

事实证明不是

在我打开了镜像源站后发现，这一切的一切都是CDN 的原因

见过多次测试，问题来源于CDN 的webp自适应的优化

`content-type:image/webp`

时候图片肯定出问题

我们可以再次修改SVG文件的 头部

去掉

`<?xml version="1.0" encoding="utf-8"?>` 

即可摆脱负优化

此时我们的`content-type:image/svg+xml`

居然这样就可以了？

过渡中的webp出现点儿问题还是在所难免得嘛

### 颜色代码

最后的最后，记录一下自己的logo的颜色代码

`#54b2fa`

真的挺好看

![我的旧LOGO的PNG格式](https://totoro.ink/img/Bcx7.png) 与

![TOTORO625图床的新LOGO](https://totoro.ink/img/totoroimg.svg)

主要是原先的字体在AI里无法加载

最为图片插入的话是使用base64插入在SVG中，不够优雅

所以索性换了字体