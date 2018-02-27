---
title: Ubuntu下配置Git
date: 2017-10-29 22:41:10
tags: Ubuntu,Git
category: Ubuntu
---
系统环境:Ubuntu 16.04 TLS
### Git安装
```
tengdw@ubuntu-16-04-lts:sudo apt-get install git
tengdw@ubuntu-16-04-lts:~/tengdwBlog$ git #安装完成后终端输入`git`确认安装没有问题
usage: git [--version] [--help] [-C <path>] [-c name=value]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

这些是各种场合常见的 Git 命令：

开始一个工作区（参见：git help tutorial）
   clone      克隆一个仓库到一个新目录
   init       创建一个空的 Git 仓库或重新初始化一个已存在的仓库

在当前变更上工作（参见：git help everyday）
   add        添加文件内容至索引
   mv         移动或重命名一个文件、目录或符号链接
   reset      重置当前 HEAD 到指定状态
   rm         从工作区和索引中删除文件

检查历史和状态（参见：git help revisions）
   bisect     通过二分查找定位引入 bug 的提交
   grep       输出和模式匹配的行
   log        显示提交日志
   show       显示各种类型的对象
   status     显示工作区状态

扩展、标记和调校您的历史记录
   branch     列出、创建或删除分支
   checkout   切换分支或恢复工作区文件
   commit     记录变更到仓库
   diff       显示提交之间、提交和工作区之间等的差异
   merge      合并两个或更多开发历史
   rebase     在另一个分支上重新应用提交
   tag        创建、列出、删除或校验一个 GPG 签名的标签对象

协同（参见：git help workflows）
   fetch      从另外一个仓库下载对象和引用
   pull       获取并整合另外的仓库或一个本地分支
   push       更新远程引用和相关的对象

命令 'git help -a' 和 'git help -g' 显示可用的子命令和一些概念帮助。
查看 'git help <命令>' 或 'git help <概念>' 以获取给定子命令或概念的
帮助。
```

### Git配置
```
tengdw@ubuntu-16-04-lts:~$ git config --global user.name "Tengdw" #""内为github用户名
tengdw@ubuntu-16-04-lts:~$ git config --global user.email "t_dw@qq.com" #""内为github主邮箱
tengdw@ubuntu-16-04-lts:~$ ssh-keygen -C 't_dw@qq.com' -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/tengdw/.ssh/id_rsa): #选择key的存储位置，可以直接回车
Created directory '/home/tengdw/.ssh'.
Enter passphrase (empty for no passphrase): #可以直接回车，也可以填写github登录密码实现免密连接
Enter same passphrase again: #确认密码
Your identification has been saved in /home/tengdw/.ssh/id_rsa.
Your public key has been saved in /home/tengdw/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:KSiRwQufdV4I26eN90AlE+QJX1IKTXdXJEDIjyMGglU t_dw@qq.com
The key's randomart image is:
+---[RSA 2048]----+
| ..oooE=B+=o+.ooo|
|. oooo+=oXo. . . |
| oo+.+.oB  o     |
|  +. ..*o.o .    |
|  . . +.S. .     |
|   .   o o       |
|          .      |
|                 |
|                 |
+----[SHA256]-----+

```

打开`~/.ssh/id_rsa.pub`文件,复制其中的所有内容，登录github-->setting-->SSH and GPG keys-->New SSH key
Title随便Key粘贴刚才复制的内容
### Git测试
```
tengdw@ubuntu-16-04-lts:~$ ssh -T git@github.com #测试能不能连接成功 如果之前输了密码系统会弹窗验证密码
Hi Tengdw! You've successfully authenticated, but GitHub does not provide shell access.
```
