---
title: "ERP网络环境要求"
date: 2021-12-20T16:45:39+08:00
# bookComments: false
# bookSearchExclude: false
---

+ VPN和内部的网络访问只能是路由或是交换的方式，不能做NAT

+ 客户端telnet到服务器后，在服务器端通过 echo $FGLSERVER 指令可以看到客户端VPN的IP

+ 从服务器发起 ping / trace 到客户端VPN的IP可以正常访问

+ 从服务器发起 telnet 客户端VPN的IP的 6400 端口可以正常访问

目前对客户端电脑的环境要求是Windows xp，Windows 7、8 ，IE版本8及以上，并且以管理员身份进入操作系统，其中IE11要设置兼容性设置，另外对于报表打印或者文档汇出，需要做如下设定：

+ 设置IE为默认浏览器  
 
+ 把ERP网址和CR网址都加入到受信任站点，将受信任安全级别调到最低 

+ 如果无法弹出网页，请尝试关闭防火墙以及其他杀毒软件