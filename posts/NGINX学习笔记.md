---
title: NGINX学习笔记
date: 2020/10/13
---

![What Is Nginx? A Basic Look at What It Is and How It Works](https://kinsta.com/wp-content/uploads/2018/03/what-is-nginx.png)



## 0. 前言

本文记录了[NGINX](https://nginx.org/)在Mac系统下的使用方法



## 1.安装

```bash
brew install nginx
```



## 2.命令

启动`nginx`

```bash
$ nginx
```

如果`nginx`已启动，那么它就可以通过使用 -s 参数的可执行命令控制。使用下列格式：

```bash
$ nginx-ssignal
```

signal 可以为下列命令之一：

stop — 直接关闭 nginx

quit — 会在处理完当前正在的请求后退出，也叫优雅关闭

reload — 重新加载配置文件，相当于重启

reopen — 重新打开日志文件 比如，等待当前子进程处理完正在执行的请求后，结束 nginx 进程



## 3.配置

```bash
$ cat /usr/local/etc/nginx/nginx.conf

worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    server {
        listen       80;
        server_name  localhost;
        location / {
            root   html;
            index  index.html index.htm;
        }
    }
    include servers/*;
}
```

### 3.1 location匹配规则

- location有四种匹配规则，优先级由高到低依次是`=` `^~` `~` `path` 。 
  - `=` 是严格全匹配
  - `^~`是前缀匹配
  - `~`是正则表达式匹配
  - `path`同`^~`但是优先级较低
- 同优先级的，匹配程度较高的先匹配。
- 匹配程度一样的,则写在前面的先匹配。

`/` `/a`  `/b` `/a/b` `/a/d` `/e` `/1` 依次匹配以下location

```nginx
location / {}  							/
location = /a {}						/a
location ^~ /a {}						/a/d
location ^~ /a/b {}					/a/b
location ~ ^/[a-z]{}				/b /e
location ~ ^/\w {}					/1
```



## 反向代理

```nginx
location /a {
  proxy_pass http://ip;
}
location /b/ {
  proxy_pass http://ip/;
}
```

上述配置会导致:

/a/x -> http://ip/a/x

/b/x -> http://ip/x



## 负载均衡

通过`upstream`配置服务器组,并通过`weight`分配权重。 然后`server`通过进行反向代理。

```nginx
upstream group1 {
  server 192.168.0.12:80 weight=1;
  server 192.168.0.12:81 weight=10;
}

server {
  location /a/ {
  	proxy_pass http://group1/;
	}
}
```



