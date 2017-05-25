---
layout: post
title: Nginx：分布式/热备
category: 技术
tags: nginx 分布式
keywords: nginx 分布式
---


#### Master


```CPP
upstream local.picture {   
       server 127.0.0.1:8081 weight=5;  
       server 127.0.0.1:8082 weight=5; # 权重越大，负载权重越大 
       server 127.0.0.1:8083 backup;   # 其它忙时，参与负载 
       server 127.0.0.1:8084 down;     # 不参与负载 
}  

server {
    listen 80; 
    server_name local.picture;
    location / { 
        proxy_pass http://local.picture;  
    }   
}
```

#### Slave1

```CPP
server {
    listen 8081;               
    server_name local.pic1;     
    location / {
        uwsgi_pass 127.0.0.1:9031;
        include uwsgi_params;  
    }
} 
```

#### Slave2

```CPP
server {
    listen 8082;               
    server_name local.pic2;     
    location / {
        uwsgi_pass 127.0.0.1:9032;
        include uwsgi_params;  
    }
} 
```

#### Slave3

```CPP
server {
    listen 8083;               
    server_name local.pic3;     
    location / {
        uwsgi_pass 127.0.0.1:9033;
        include uwsgi_params;  
    }
} 
```

#### Slave4

```CPP
server {
    listen 8084;               
    server_name local.pic4;     
    location / {
        uwsgi_pass 127.0.0.1:9034;
        include uwsgi_params;  
    }
} 
```
