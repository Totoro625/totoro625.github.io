---
title: APlayer and DPlayer
date: 2017-05-22 21:53:10
tags: hexo
category: bash
---
作为一个热爱生活的土木男
当然要多听听音乐看看视频啦
很是喜欢[DIYgod][1]的两个播放器[APlayer][2]和[DPlayer][3]
# Typecho里面的插件并不是很智能
所以我得记录下他们的用法


<!--more-->


## APlayer
`[[player url="http://x.x.x.com/xxx.mp3" artist="Someone" title="Title" showlrc="false"/]]`

网易云音乐：
`[[player id="29947420"/]]`

属性说明
`url: mp3文件的链接，必需
lrc: 歌词的lrc链接，非必需
lrcoffset: 歌词整体提前时间（ms）若这个值为负数则为歌词整体延后的时间
title: 歌曲的标题，若值为空则显示 Unknown
artist: 歌曲的艺术家，若值为空则显示 Unknown
cover: 封面图片链接，非必需`

### 我会怎么用？
网易云音乐`[[player id="29947420" autoplay="true" cover="http://xxx.com/xxx.png"/]]`
注：网易云封面用的是http
其他音乐`[[player url="http://xxx.com/xxx.mp3" artist="Someone" title="Title" autoplay="true" cover="http://xxx.com/xxx.png"/]]`

[player url="https://X.X.X/music/34723470.mp3" artist="囧菌" title="东京不太热" autoplay="false" cover="https://X.X.X/music/34723470.jpg"/]
`[[player url="https://X.X.X/music/34723470.mp3" artist="囧菌" title="东京不太热" autoplay="false" cover="https://X.X.X/music/34723470.jpg"/]]`
注：关闭了自动播放

(还是wordpress插件丰富啊，[Hermit-X][4]很简单,网易云也能完美支持ssl）

## DPlayer
我是这样用的
`[[dplayer url="http://xxx.com/xxx.mp4" pic="http://xxx.com/xxx.jpg" danmu="false" autoplay="true"/]]`
谁让我不会搭弹幕呢
[dplayer url="https://X.X.X/miku/freely-tomorrow.mp4" pic="https://X.X.X/miku/freely-tomorrow.png" danmu="false" autoplay="false"/]
`[[dplayer url="https://X.X.X/miku/freely-tomorrow.mp4" pic="https://X.X.X/miku/freely-tomorrow.png" danmu="false" autoplay="false"/]]`
注：关闭了自动播放

  [1]: https://www.anotherhome.net/
  [2]: https://github.com/DIYgod/APlayer
  [3]: https://github.com/DIYgod/DPlayer
  [4]: https://github.com/liwanglin12/Hermit-X