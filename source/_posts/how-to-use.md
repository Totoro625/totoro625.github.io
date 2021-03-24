---
title: 如何使用hexo
date: 2017-06-20 02:38:10
tags: hexo
category: bash
---
# 常用命令
# fontawesome图标

``` 
<!-- 1.3倍大小 -->
<i class="icon icon-book icon-lg"></i>
<!-- 2倍大小 -->
<i class="icon icon-book icon-2x"></i>
<!-- 3倍大小 -->
<i class="icon icon-book icon-3x"></i>
<!-- 4倍大小 -->
<i class="icon icon-book icon-4x"></i>
<!-- 5倍大小 -->
<i class="icon icon-book icon-5x"></i>
<!-- 5px右边距 -->
<i class="icon icon-book icon-pr"></i>
<!-- 5px左边距 -->
<i class="icon icon-book icon-pl"></i>
```
<!-- more -->
# 自定义页面

```
hexo new page 链接后缀
```

```
---
layout: page 
title: 关于我
description: 一些介绍
comments: false
---
```

# 文章页

`hexo new "链接后缀"` 

```
---
title: IOS 
date: 2017-06-20 07:30:10
tags: 
  - hyperapp
  - docker
  - EFB
comments: false
---
```



`<!-- more -->  `设置文章摘要

`hexo deploy` 推送到远程

`tags: [a, b, c]`设置标签

`hexo clean` 清除缓存

`hexo generate`生成新的页面

# 还可能会用到

[makedown与html互转][1]


[1]: http://www.atool.org/html2markdown.php