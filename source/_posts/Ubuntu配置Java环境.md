---
title: Ubuntu配置Java环境
date: 2017-11-02 23:17:26
tags: Linux Ubuntu Java
category: Ubuntu
---
环境：Ubuntu 16.04 TLS
先检查有无Java环境`java -version` ,如果以前没装过会输出
```
tengdw@tengdw-Ubuntu:~$ java -version
程序 'java' 已包含在下列软件包中：
 * default-jre
 * gcj-5-jre-headless
 * openjdk-8-jre-headless
 * gcj-4.8-jre-headless
 * gcj-4.9-jre-headless
 * openjdk-9-jre-headless
请尝试：sudo apt install <选定的软件包>
```
去[Oracle官网下载JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html),选linux-64.tar.gz格式的
解压后会得到一个前缀为jdk的目录，复制目录到`/usr`下（也可以放在其他目录）
编辑`/etc/profile`文件，导入Java环境变量，使用Ubuntu自带的gedit编辑器
```
tengdw@tengdw-Ubuntu:~$ sudo gedit /etc/profile
#profile内容如下:
# /etc/profile: system-wide .profile file for the Bourne shell (sh(1))
# and Bourne compatible shells (bash(1), ksh(1), ash(1), ...).

if [ "$PS1" ]; then
  if [ "$BASH" ] && [ "$BASH" != "/bin/sh" ]; then
    # The file bash.bashrc already sets the default PS1.
    # PS1='\h:\w\$ '
    if [ -f /etc/bash.bashrc ]; then
      . /etc/bash.bashrc
    fi
  else
    if [ "`id -u`" -eq 0 ]; then
      PS1='# '
    else
      PS1='$ '
    fi
  fi
fi

if [ -d /etc/profile.d ]; then
  for i in /etc/profile.d/*.sh; do
    if [ -r $i ]; then
      . $i
    fi
  done
  unset i
fi
#在最后加上
export JAVA_HOME=/usr/jdk1.8.0_151 #/usr/jdk1.8.0_151为jdk的主目录
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JRE_HOME/lib
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
```
**使用`source /etc/profile`是更改的profile文件生效**
再次使用`java -version`验证
```
tengdw@tengdw-Ubuntu:~$ source /etc/profile
tengdw@tengdw-Ubuntu:~$ java -version
java version "1.8.0_151"
Java(TM) SE Runtime Environment (build 1.8.0_151-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.151-b12, mixed mode)
```

