---
title: 关于java环境重装的问题
tags:
  - Java
categories:
  - Java
summary: java环境重装踩坑随笔
img: /images/java重装/1.jpg
abbrlink: 9eb3
date: 2021-03-01 13:00:00
updated: 2021-03-01 13:00:00

---

## 前言

>由于上次加装固态硬盘，导致java环境失效，所以将其卸载重装。 

-----

## java环境变量配置

	#添加系统变量
	
	新建变量名：JAVA_HOME 变量值：java安装路径，如F:\Program Files\Java\jdk1.8.0_221
	
	新建变量名：classpath 变量值：.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar (前面的.不要漏掉)
	
	在Path变量中加入变量值：.;%JAVA_HOME%\bin;（加在最前面）


-------

## 报错 

	Error: opening registry key 'Software\JavaSoft\Java
	Runtime Environment' Error: could not find java.dll
	Error: Could not find Java SE Runtime Environment. 

-----

## 解决

* Windows键+r 输入regedit打开注册表
* 依次进入目录HKEY_LOCAL_MACHINE----->SOFTWARE----->JavaSoft----->Java Development Kit
* 查看文件，将旧的文件删除
* 进入C:\Windows\System32找到java.exe和javaw.exe并将它们删除（地址栏中输入**%SystemRoot%\system32**也可进入该路径）
* cmd命令行运行java -version验证环境是否配置成功。

   



