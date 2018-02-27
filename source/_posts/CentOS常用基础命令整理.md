---
title: CentOS常用基础命令整理
date: 2017-10-15 15:05:15
tags: Linux
---

CentOS（Community Enterprise Operating System，中文意思是：社区企业操作系统）是Linux发行版之一，它是来自于Red Hat Enterprise Linux依照开放源代码规定释出的源代码所编译而成。由于出自同样的源代码，因此有些要求高度稳定性的服务器以CentOS替代商业版的Red Hat Enterprise Linux使用。两者的不同，在于CentOS并不包含封闭源代码软件。
```
Linux命令格式：命令 [选项] [参数]
```
## ls
查询目录中的内容：ls [选项] [文件或目录]
选项：
- -a 显示所有文件，包括隐藏文件
- -t 显示所有文件，并以更改时间排序
- -l    显示详细信息(ls -l = ll)
- -h    人性化显示文件大小
- -i    显示inode
- -s   显示所有文件，并且以区块为单位的形式表示文件和目录大小
- -R   显示所有文件，并将所有子目录的文件都列出来

## mkdir
建立目录：mkdir -p [目录名] 
- -p 递归创建(例：mkdir -p abc/def)
- 命令英文原意：make directories

## cd
切换所在目录：cd  [目录]
- 命令英文原意：change directory
- cd ~    进入当前用户的home目录
- cd –    进入上次目录
- cd ..    进入上一级目录
- cd .    进入当前目录

### 文件权限
- Linux靠权限来识别文件类型
- –rw-r-r–
- –文件类型（-文件 &nbsp; d 目录 &nbsp; i 软链接文件）

| rw- | r– | r- |
|:------|:-------|:------|
|u所有者| g所属组 | o其他人 |
| r读 | w写 | x执行 |

### 相对路径与绝对路径
- 相对路径：参照当前所在目录，进行查找 
> 如：[root@valleyecho ~]# cd ../var/www/html/
- 绝对路径：从根目录开始指定，一级一级递归查找。在任何目录下都能进入指定位置 
> 如：[root@valleyecho ~]# cd /var/

## rmdir
删除空目录：rmdir [目录名]
命令英文原意：remove empty directories
*只能删除空目录*

## rm -rf
删除文件或目录：rm -rf [文件或目录]
命令英文原意：remove 
选项：
- -r 删除目录
- -f 强制（force）

## cp
复制命令：cp [选项] [原文件或目录] [目标目录]
命令英文原意：copy 
选项：
- -r 复制目录
- -p 连带文件属性复制
- -d 若源文件是链接文件，则复制链接属性
- -a 相当于 -pdr

## mv
剪切命令：mv  [原文件或目录] [目标目录]
命令英文原意：move
**mv  [原文件名或目录名] [新的文件名或目录名]  #实现重命名操作**

## ln
链接命令：ln  [原文件] [目标文件]
命令英文原意：link
功能描述：生成链接文件 
选项：
- -s 创建软链接


### 硬链接与软链接

- 硬链接特征：
 1. 拥有相同的i节点和存储block块，可以看做是同一个文件
 2. 可通过i节点识别
 3. 不能跨分区
 4. 不能针对目录使用
- 软链接特征：
 1. 类似Windows快捷方式
 2. 软链接拥有自己的i节点和block块，但是 数据块中只保存源文件的文件名和i节点号，并没有实际的文件数据
 3. lrwxrwxrwx &nbsp; l 软链接 软链接文件权限都为rwxrwxrwx
 4. 修改任意文件，另一个都改变
 5. 删除原文件，软链接不能使用

## locate
文件搜索命令：locate  [文件名]
在后台数据库中按文件名搜索，搜索速度更快
- /var/lib/mlocate   &nbsp;   #locate命令所搜索的后台数据库
- updatedb  &nbsp; #更新数据库

## whereis
搜索命令的命令：whereis
- whereis 命令名  &nbsp;  #搜索命令所在路径及帮助文档所在位置
- -b    只查找可执行文件
- -m    只查找帮助文件

## which
搜索命令的命令：which
- which    命令名  &nbsp;  #搜索命令所在路径及别名

## find
搜索命令：find
- find [搜索范围] [搜索条件]    #搜索文件
- find / -name install.log
*避免大范围搜索，会非常耗费系统资源*
*find是在系统当中搜索符合条件的文件名。如果需要匹配使用通配符，通配符是完全匹配的* 
Linux中的通配符：
 1. \* 匹配任意内容
 2. ? 匹配任意一个字符
 3. [] 匹配任意一个中括号内的字符
- find /toot -iname install.log  &nbsp;  #不区分大小写
- find /root -user root  &nbsp;  #按照所有者搜索
- find /root -nouser  &nbsp;  #查找没有所有者的文件
- find /var/log/ -mtime +10  &nbsp;  #查找10天前修改的文件
> -10    10天内修改的文件 
> 10    10天当天内修改的文件 
> +10    10前内修改的文件 
> atime    文件访问时间 
> ctime    改变文件属性 
> mtime    修改文件内容
- find . -size 25k    #查找文件大小是25kb的文件（小k，大M）
> -25  &nbsp;  小于25kb的文件 
> 25   &nbsp;  等于25kb的文件 
> +25  &nbsp; 大于25kb的文件
- find . -inum 262422  &nbsp;  #查找i节点是262422的文件
- find /etc -size +20k -a -size -50k  &nbsp;  #查找/etc/目录下，大于20kb并且小于50kb的文件
> -a  &nbsp;  and    逻辑与，两个条件都满足 
> -o  &nbsp;  or    逻辑或，两个条件满足一个即可
- find /etc -size +20k -a -size -50k -exec ls -lh {} \;
查找/etc/目录下，大于20kb并且小于50kb的文件,并显示详细信息 
-exec 与 {} \;是配对使用的，固定格式,对操作结果执行操作

## grep
搜索字符串命令：grep
grep [选项] 字符串 文件名  &nbsp;  #在文件当中匹配符合条件的字符串
选项：
- -i 忽略大小写
- -v 排除指定字符串

## man
帮助命令
- man [选项] [参数]  &nbsp;  #获取指定命令的帮助
- 例：man ls  &nbsp;  #查看ls的帮助
- [命令] -  -help  &nbsp;  #获取命令选项的帮助 例：ls - -help

## 压缩与解压缩命令
Linux常用压缩格式：.zip  &nbsp;  .gz  &nbsp;  .bz2
常用压缩格式：.tar.gz  &nbsp;  .tar.bz2

## .zip格式压缩与解压缩
- zip [压缩文件名.zip] [源文件名]  &nbsp;  #压缩文件
- zip -r [压缩文件名.zip] [源目录]  &nbsp;  #压缩目录
- unzip [压缩文件名]  &nbsp;  #解压缩.zip文件

## .gz格式压缩
- gzip [源文件名]  &nbsp;  #压缩为.gz格式的压缩文件，源文件会消失
- gzip -c [源文件名] > [压缩文件名.gz]  &nbsp;  #压缩为.gz格式，源文件保留
- gzip -r [目录名]  &nbsp;  #压缩目录下所有的子文件，但是不能压缩目录

## .bz2格式压缩与解压缩
- bzip2 [源文件名]  &nbsp;  #压缩为.bz2格式，不保留源文件
- bzip2 -k [源文件名]  &nbsp;  #压缩之后保留原文件
- 注意：bzip2命令不能压缩目录
- bzip2 -d [压缩文件名]  &nbsp;  #解压缩，-k保留压缩文件
- bunzip2 [压缩文件名]  &nbsp;  #解压缩，-k保留压缩文件

## 打包与解打包
- tar -cvf  [打包文件名.tar]  [源文件名]
选项：
- -c  打包
- -v  显示过程
- -f  指定打包后的文件名
- tar -xvf [打包文件名]  #-x   解打包

## .tar.gz压缩与解压缩
- .tar.gz格式是先打包为.tar格式，在压缩为.gz格式
- tar -zcvf [(可加其他目录)压缩包名.tar.gz] [源文件] &nbsp; #-z   压缩为 .tar.gz格式
- tar -zxvf [压缩包名.tar.gz] &nbsp; #-x  解压缩.tar.gz格式
- tar -zxvf [压缩包名.tar.bz2] -C [目标目录] &nbsp; #解压文件到目标目录

## .tar.bz2压缩与解压缩
- tar -jcvf [压缩包名.tar.bz2] [源文件] &nbsp; #-z   压缩为 .tar.bz2格式
- tar -jxvf [压缩包名.tar.bz2] &nbsp; #-x    解压缩.tar.bz2格式

## 其他命令
- w  &nbsp;  #查看用户登录信息（详细）
- who [用户名] &nbsp; #查看用户登录信息
- last &nbsp; #查询当前登录和过去登录的用户信息
- lastlog &nbsp; #查看所有用户的最后一次登录时间


