---
title: 腾讯云香港轻量云服务器使用笔记
tags:
- v2ray
- Linux
categories:
- 笔记
- 云服务器
- Linux

summary: 轻量云服务器搭建...
img: /images/v2ray/9.png
abbrlink: 73f7
date: 2021-02-28 13:00:00
updated: 2021-02-28 13:00:00
 
---

## v2ray搭建

1.购买**香港**地区的轻量云服务器（Centos7+）后，开启全部的tcp和udp端口。如下图  

![](/images/v2ray/1.png)
![](/images/v2ray/2.png)

2.远程登录云服务器，运行如下命令  

	sudo -i
	bash <(curl -Ls https://blog.sprov.xyz/v2-ui.sh)

![](/images/v2ray/3.png)
![](/images/v2ray/4.png)
![](/images/v2ray/5.png)  


3.安装完成后输入云服务器公网ip:65432进入后台，用户名密码都为admin
![](/images/v2ray/6.png)
4.进入后台后添加账号，然后导入v2ray即可。
![](/images/v2ray/7.png)
![](/images/v2ray/8.png)

------

## 部署谷歌bbr加速
### 升级Centos7内核
内核不可低于 4.9.0   
1.输入如下命令查看内核版本

	uname -r
2.分别输入如下命令安装内核

	sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
	sudo rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
	sudo yum --enablerepo=elrepo-kernel install kernel-ml -y
3.显示grub2菜单中的所有条目：

	sudo egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \'

4.从3命令显示的条目中选择要更换的内核版本，设第一个条目为0正好是我们需要的内核。则输入如下命令（第二个就为1，以此类推）

	sudo grub2-set-default 0

5.重启系统

	sudo shutdown -r now
6.查看内核是否更换成功(不低于4.9.0)
	
	uname -r

----
### 启用BBR加速

1.输入如下命令修改配置

	echo 'net.core.default_qdisc=fq' | sudo tee -a /etc/sysctl.conf
	echo 'net.ipv4.tcp_congestion_control=bbr' | sudo tee -a /etc/sysctl.conf
	sudo sysctl -p

2.确认已启用bbr

	sudo sysctl net.ipv4.tcp_available_congestion_control
3.验证

	lsmod | grep bbr
结果类似如下即成功开启bbr
	
	tcp_bbr                16384  0
  


