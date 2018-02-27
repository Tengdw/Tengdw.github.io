---
title: Ubuntu依赖关系问题
date: 2017-11-02 23:17:09
tags: Ubuntu,Linux
category: Ubuntu
---
系统版本：16.04 LTS
使用`dpkg -i filename.deb`安装软件出现依赖关系问题
```
sudo apt-get update #更新本地源列表
sudo apt-get install #执行后终端会提示前面安装的filename.deb缺少的库
sudo apt-get -f install #执行后就能通过搜索找到安装的软件啦
