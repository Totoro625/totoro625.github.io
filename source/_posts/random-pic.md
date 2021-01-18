---
title: 随机图片api与一言(hitokoto)api
date: 2018-01-15 23:27:00
update: 2018-01-16 19:25:00
tags: api
category: bash
---

## 想法来源

日常逛博客看到[未闻花名的博客](https://snrat.com/) 头部图片一直在变换，F12看到用了一个[PHP](https://api.ikmoe.com/shawu-rand-background.php)来加载随机图片。

~~不得不感慨一句和泉沙雾最可爱了，你们不要跟我抢~~
<!--more-->
顺着链接爬到了[月宅酱](https://ikmoe.com)的[漫月api](https://random.ikmoe.com/) ，看到他每个月[维护所需的花费](https://ikmoe.com/5722.html)那么巨大，我还是撸一个自己用吧

（其实主要是可用的接口过少）目前有一个[公开接口](https://api.ikmoe.com/moeu-rand-background.php) 以及一个博文内[隐藏接口-小忍壁纸](https://api.ikmoe.com/xiao-ren-api.php) 还有我爬取到的[和泉沙雾](https://api.ikmoe.com/shawu-rand-background.php) 。~~和泉沙雾最可爱~~

顺手爬了一下月宅酱的和泉沙雾api接口内的全部图片，[一共50张(相册)](https://img.totoro.ink/album/m7n)

由于很久没有写PHP的原因，月宅酱的漫月api也没有公开源码我打算照抄一下一言(hitokoto)api的源码来写这个随机图片的api

## 寻找一言API

lwl12的 [LWL-Hitokoto API（一言-纯净API）](https://blog.lwl12.com/read/hitokoto-api.html) 未开源

狂放的 [[开源]一言纯净API](https://www.iknet.top/568.html)  [开源](https://github.com/kfangf/hitokoto) 也很好用

在狂放的评论区找到了后宫学长的超简洁版api [使用“一言”为博客增加文雅气息](https://haremu.com/p/94) 差不多7行代码就搞定

- 新建一个文件夹，名为：hitokoto
- 复制以下代码，保存为 index.php 文件，并放置上面的文件夹中

```
<?php
//获取句子文件的绝对路径
$path = dirname(__FILE__);
$file = file($path."/hitokoto.txt");
//随机读取一行
$arr  = mt_rand( 0, count( $file ) - 1 );
$hitokoto  = trim($file[$arr]);
//输出内容
echo $hitokoto;
?>
```

- 把你的一言名言写进 hitokoto.txt 文件中，一句话一行，并放置上面的文件夹中

**示例：**

> 有你在的日子才是我的日常。
> 夹在我女友与前女友与青梅竹马间的果然是修罗场！
> 既然如此，就再努力一次吧。别在这里愁眉不展，也不要再自欺欺人，重新来过！
> 比自己,比梦想更重要的东西永远都存在着...
> 嘛，那又怎么样呢？

就这样就可以了

## 改造成随机图片API

那么一言api有了之后，我们该如何改造他变成一个随机图片api呢？

http的303跳转的作用是：对于POST请求，它表示请求已经被处理，客户端可以接着使用GET方法去请求Location里的URI

这样我们可以用

```
if (isset($url)) 
{ 
Header("HTTP/1.1 303 See Other"); 
Header("Location: $url"); 
exit; 
} 
```

来进行跳转

在原有的API基础上修改一下就是

```
<?php
//获取句子文件的绝对路径
$path = dirname(__FILE__);
$file = file($path."/pics.txt");
//随机读取一行
$arr  = mt_rand( 0, count( $file ) - 1 );
$url  = trim($file[$arr]);
//输出内容
#echo $url;
//303跳转
if (isset($url)) 
{ 
Header("HTTP/1.1 303 See Other"); 
Header("Location: $url"); 
exit; 
} 
?>
```

新建一个pics.txt写入你所需要的随机展示的图片即可食用



我的和泉沙雾是

```
https://img.totoro.ink/images/2018/01/15/mVKl.png
https://img.totoro.ink/images/2018/01/15/muqM.jpg
https://img.totoro.ink/images/2018/01/15/mj3d.png
https://img.totoro.ink/images/2018/01/15/mqJ9.jpg
https://img.totoro.ink/images/2018/01/15/mHNR.png
https://img.totoro.ink/images/2018/01/15/miMI.jpg
https://img.totoro.ink/images/2018/01/15/m291.jpg
https://img.totoro.ink/images/2018/01/15/m4Ca.jpg
https://img.totoro.ink/images/2018/01/15/mnng.jpg
https://img.totoro.ink/images/2018/01/15/mZEP.jpg
https://img.totoro.ink/images/2018/01/15/mfzT.jpg
https://img.totoro.ink/images/2018/01/15/mXTU.jpg
https://img.totoro.ink/images/2018/01/15/mW33.jpg
https://img.totoro.ink/images/2018/01/15/mMFV.jpg
https://img.totoro.ink/images/2018/01/15/msNJ.jpg
https://img.totoro.ink/images/2018/01/15/mDMs.jpg
https://img.totoro.ink/images/2018/01/15/mAek.jpg
https://img.totoro.ink/images/2018/01/15/m3C7.jpg
https://img.totoro.ink/images/2018/01/15/monX.jpg
https://img.totoro.ink/images/2018/01/15/mhBy.jpg
https://img.totoro.ink/images/2018/01/15/mPzp.jpg
https://img.totoro.ink/images/2018/01/15/mGTn.jpg
https://img.totoro.ink/images/2018/01/15/mEoS.jpg
https://img.totoro.ink/images/2018/01/15/mBFE.jpg
https://img.totoro.ink/images/2018/01/15/mr8i.jpg
https://img.totoro.ink/images/2018/01/15/mpwK.jpg
https://img.totoro.ink/images/2018/01/15/mmeh.jpg
https://img.totoro.ink/images/2018/01/15/meyj.jpg
https://img.totoro.ink/images/2018/01/15/m7n0.jpg
https://img.totoro.ink/images/2018/01/15/mxBA.jpg
https://img.totoro.ink/images/2018/01/15/mLg8.jpg
https://img.totoro.ink/images/2018/01/15/mJTl.jpg
https://img.totoro.ink/images/2018/01/15/mFoM.jpg
https://img.totoro.ink/images/2018/01/15/m00d.jpg
https://img.totoro.ink/images/2018/01/15/9U89.jpg
https://img.totoro.ink/images/2018/01/15/9OwR.jpg
https://img.totoro.ink/images/2018/01/15/9K7I.jpg
https://img.totoro.ink/images/2018/01/15/9gy1.jpg
https://img.totoro.ink/images/2018/01/15/9cZa.jpg
https://img.totoro.ink/images/2018/01/15/9S1g.jpg
https://img.totoro.ink/images/2018/01/15/9IgP.jpg
https://img.totoro.ink/images/2018/01/15/9CHT.jpg
https://img.totoro.ink/images/2018/01/15/9yhU.jpg
https://img.totoro.ink/images/2018/01/15/9t03.jpg
https://img.totoro.ink/images/2018/01/15/95bV.jpg
https://img.totoro.ink/images/2018/01/15/9kwJ.jpg
https://img.totoro.ink/images/2018/01/15/9N7s.jpg
https://img.totoro.ink/images/2018/01/15/9btk.jpg
https://img.totoro.ink/images/2018/01/15/9VZ7.jpg
https://img.totoro.ink/images/2018/01/15/9u1X.jpg
https://img.totoro.ink/images/2018/01/15/9qcy.jpg
https://img.totoro.ink/images/2018/01/15/9THp.jpg
https://img.totoro.ink/images/2018/01/15/9Hhn.jpg
https://img.totoro.ink/images/2018/01/15/92vS.jpg
https://img.totoro.ink/images/2018/01/15/9abE.jpg
https://img.totoro.ink/images/2018/01/15/94si.jpg
```

就这样，一份随机图片api就写好了，不用受到别人的约束，自己写的才是最好用的。

## 用法

我在[我的图床](https://img.totoro.ink)上展示了几个[api的用法](https://img.totoro.ink/page/api) ，由于写的不是很直观，我在这里就直接贴出

### 和泉沙雾50张

来源于相册[和泉沙雾](https://img.totoro.ink/album/m7n)，可以使用<https://pan.totoro.ink/code/php/random-pics/shawu.php>这个链接调用随机图片

调用地址：https://pan.totoro.ink/code/php/random-pics/shawu.php

### 和泉沙雾193张

图片来源[漫月API](https://random.ikmoe.com/)，我把它们另存于相册[和泉沙雾2](https://img.totoro.ink/album/rby)

调用地址：https://pan.totoro.ink/code/php/random-pics/shawu2.php

### 博客daily_pic

[我的博客](https://totoro.ink/)首页随机图片

调用地址：https://pan.totoro.ink/code/php/random-pics/daily_pic.php

复制上面的地址到你需要显示图片的地方

例如：background:url(https://pan.totoro.ink/code/php/random-pics/daily_pic.php);

其他：随时跑路，食用请自建，搭建方式[随机图片api与一言api](https://totoro.ink/random-pic.html) 

## 使用到的图片

具体使用图片明细请在[随机图片api](https://img.totoro.ink/page/api)查看