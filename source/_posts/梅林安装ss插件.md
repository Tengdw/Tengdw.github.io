---
title: 梅林安装ss插件
date: 2017-10-14 17:39:47
tags: 路由器
category: 硬件玩家
---

前几天给K3刷了梅林固件（LEDE用起来太麻烦了），由于现在严查番羽墙梅林软件中心的ss插件被迫下架了，所以需要手动安装
 1. 先启用梅林的SSH：系统管理->系统设置->SSH Daemon->Allow SSH Port Forwarding 选是再点击应用本页面设置
 2. 用SSH客户端登录（地址：路由器的默认网关；用户名：默认是admin；密码：路由器的管理密码）然后一句一句执行下面命令
```
cd /tmp
wget http://valleyecho-1252960615.cosgz.myqcloud.com/shadowsocks.tar.gz
tar -zxvf /tmp//shadowsocks.tar.gz    #解压
chmod +x /tmp/shadowsocks/install.sh    #授予执行权限
sh /tmp/shadowsocks/install.sh    #执行
```

