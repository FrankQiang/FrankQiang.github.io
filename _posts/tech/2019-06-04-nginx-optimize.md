---
layout: post
title: Nginx：优化
category: 技术
tags: nginx 优化
keywords: nginx 优化
---


#### 常用优化


```CPP

nginx 进程数，建议按照 cpu 数目来指定

worker_processes 8 

为每个进程分配 cpu，将 8 个进程分配到 8 个 cpu 中

worker_cpu_affinity 00000001 00000010 00000100 00001000 00010000 00100000 01000000 10000000

一个 nginx 进程打开的最多文件数目, 理论值应该是最多打开文件数（ulimit -n）与 nginx 进程数相除, 但是 nginx 分配请求并不是那么均匀, 所以最好与 ulimit -n 的值保持一致.

worker_rlimit_nofile 65535

使用 epoll 的 I/O 模型

use epoll

每个进程允许的最多连接数, 理论上每台 nginx 服务器的最大连接数为`worker_processes`*worker_connections.

worker_connections 65535

keepalive 超时时间

keepalive_timeout 60

```


#### 示例

```CPP

user nginx;
worker_processes 2;
worker_cpu_affinity 00000001 00000010;
worker_rlimit_nofile    65535;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
	worker_connections 65535;
	multi_accept on;
	use epoll;
}

http {
    log_format main '$remote_addr - $remote_user [$time_local] "$request" $http_host '
                    '$status $request_length $body_bytes_sent "$http_referer" '
                    '"$http_user_agent"  $request_time $upstream_response_time';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   60;
    types_hash_max_size 2048;
    client_max_body_size 15M;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;
    include /etc/nginx/conf.d/*.conf;


}

```
