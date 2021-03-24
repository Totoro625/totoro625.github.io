---
title: chevereto 图床的迁移
date: 2017-12-02 20:36:10
tags: network
category: bash
---

[chevereto](https://chevereto.com) 是一个知名度较高的个人图床程序，[GitHub提供了免费版](https://github.com/Chevereto/Chevereto-Free)的同时也有功能更加丰富的收费版可供选择。当然[价格嘛](https://chevereto.com/pricing)。。。暂时买不起

这不，我连托管服务器的费用都快负担不起了，需要把  chevereto 整个网站数据给迁移走。在这当中遇到了很多坑人的地方，我就来详细记录一二。

以下均以免费版为示例

<!-- more -->

### 错误的尝试

首先我尝试了整站数据直接打包转移，很明显，这样做是不行的，直接转移之后图床就真的只能作为一个离线图床进行使用了，账户均无法登录成功

其次我尝试了本地安装完成之后数据库覆盖，也失败了

![特殊码](https://totoro.ink/img/chevereto.png)

随后在仪表盘中找到了一个名为`Crypt salt 特殊码`的字符串，在不同的操作系统上，本函数的行为不同。这也导致了直接转移网站数据的失败。

### 正确的方法

最终我找到的解决方法是：

1、独立安装新的网站程序

2、将图片数据导入新的平台

3、将图片对应的数据库表`chv_images`进行导入，这样新的网站也能查看到之前的图片数据

4、将相册对应的数据库表`chv_albums`进行导入，以使用原先的相册功能

5、将设置对应的数据库表`chv_settings`的`chevereto_version_installed`与`crypt_salt`字段对应的数据进行记录（/保存），例如我的是`1.0.9`与`46 ******`，然后将`chv_settings`表导入，恢复原先的设置功能

6、将相册对应的数据库表`chv_users`进行导入，以恢复用户所有权的更正

​	注：第六项也可以通过仅仅修改`user_date` `user_date_gmt` `user_image_count` `user_album_count` `user_content_views` 字段达到恢复用户所有权更正的目的。（不进行修改的话会出现图片ower是你，个人管理页面不存在的问题）

顺便说一下，chevereto 的Nginx 设置应包含如下设置

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
```

以便正常使用

### 目前尚存的问题

依旧是有一些问题没能解决：

1、用户的头像无法恢复

2、用户设置的自定义背景无法设置

以上设置在数据库中修改均会自动失效，尚未找到解决办法

