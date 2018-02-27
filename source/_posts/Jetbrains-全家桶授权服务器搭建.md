---
title: Jetbrains 全家桶授权服务器搭建
date: 2017-11-02 23:17:45
tags: IDEA Linux
category: 服务器
---
授权服务器地址：**http://idea.valleyecho.cn**
用法：软件内-->Help-->Register...-->License server-->License server address 填入上面的地址就行了
IntelliJ IDEA、PHPStrom、PyCharm、WebStrom等IDE都是由Jetbrains公司开发的。
搭建环境：CentOS 7.3
软件下载地址：[点我下载](https://valleyecho-1252960615.cosgz.myqcloud.com/IntelliJIDEALicenseServer%280.0.0.0_1017%29.zip) 软件作者是 [Lanyu](https://blog.lanyus.com)，更多的用法请去他的博客看
压缩包解压有多个环境运行版本的一般用后缀 _amd64 的就行了，上传到服务器上。我这里放在`/root`目录的
```
cd /root
mv IntelliJIDEALicenseServer_linux_amd64 IdeaServer #名字太长了改下名
chmod +x ./IdeaServer #授予执行权限
./IdeaServer #运行
#运行成功后显示以下信息
[root@VM_129_4_centos ~]# ./IdeaServer 
2017/11/02 22:46:06 *************************************************************
2017/11/02 22:46:06 ** IntelliJ IDEA License Server                            **
2017/11/02 22:46:06 ** by: ilanyu                                              **
2017/11/02 22:46:06 ** http://www.lanyus.com/                                  **
2017/11/02 22:46:06 ** Alipay donation: lanyu19950316@gmail.com                **
2017/11/02 22:46:06 ** Please support genuine!!!                               **
2017/11/02 22:46:06 ** listen on 0.0.0.0:1017...                               **
2017/11/02 22:46:06 ** You can use http://127.0.0.1:1017 as license server     **
2017/11/02 22:46:06 *************************************************************
2017/11/02 22:46:06 listen tcp :1017: bind: address already in use
```
上面的`http://127.0.0.1:1017`就是 License server address
`Ctrl+c`退出程序，我们想作为授权服务器就必须要该软件一直运行在后台，这里需要`screen`命令,可以用`screen -v`检测版本的方式来确认有无该命令
```
screen -dmS IdeaServer ./IdeaServer #让IdeaServer在后台运行，可以用`top`来查看程序有没有在后台运行
```
最后添加开机启动
```
vim /etc/rc.local
#在文件末尾添加
cd /root/
screen -dmS IdeaServer ./IdeaServer
```
现在还只能在服务器本地使用，通过Nginx反向代理`http://127.0.0.1:1017`我们就能在远程实现激活啦。由于我用的是面板，配置反向代理是傻瓜式的这里就不说了，贴上 idea.valleyecho.cn 的Nginx配置文件
```
server
{
    listen 80;
    server_name console.valleyecho.cn;
    index index.php index.html index.htm default.php default.htm default.html;
    root /www/wwwroot/console;
    
    #SSL-START SSL相关配置，请勿删除或修改下一行带注释的404规则
    #error_page 404/404.html;
    #SSL-END
    
    #ERROR-PAGE-START  错误页配置，可以注释、删除或修改
    error_page 404 /404.html;
    error_page 502 /502.html;
    #ERROR-PAGE-END
    
    #PHP-INFO-START  PHP引用配置，可以注释或修改

	#PROXY-START
    location ~ /purge(/.*) { 
        proxy_cache_purge cache_one $host$request_uri$is_args$args;
        #access_log  /www/wwwlogs/console.valleyecho.cn_purge_cache.log;
    }
    location / 
    {
        proxy_pass http://localhost:20170;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $remote_addr;
        #proxy_cache cache_one;
        #proxy_cache_key $host$request_uri$is_args$args;
        #proxy_cache_valid 200 304 301 302 1h;
        add_header X-Cache $upstream_cache_status;
        
        expires 12h;
    }
    
    location ~ .*\.(php|jsp|cgi|asp|aspx|flv|swf|xml)?$
    { 
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_pass http://localhost:20170;
        
    }
    #PROXY-END

	include enable-php-54.conf;
    #PHP-INFO-END
    
    #REWRITE-START URL重写规则引用,修改后将导致面板设置的伪静态规则失效
    include /www/server/panel/vhost/rewrite/console.valleyecho.cn.conf;
    #REWRITE-END
    
    #禁止访问的文件或目录
    access_log  /www/wwwlogs/console.valleyecho.cn.log;
}
```

