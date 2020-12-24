---
title: "Nginx 支持Websocket"
tags: ""
---

# Nginx 支持websocket

## nginx.conf

```nginx
http {
    map $http_upgrade $connection_upgrade {
        default upgrade;
        '' close;
    }
    
    /* 
    map指令的作用：
    该作用主要是根据客户端请求中$http_upgrade 的值，来构造改变$connection_upgrade的值，即根据变量$http_upgrade的值创建新的变量$connection_upgrade，
    创建的规则就是{}里面的东西。其中的规则没有做匹配，因此使用默认的，即 $connection_upgrade 的值会一直是 upgrade。然后如果 $http_upgrade为空字符串的话，
    那值会是 close。
    */        
        
    location {
        proxy_pass http://serverhost.com;
        proxy_http_version 1.1;
        proxy_set_header Host $host:$server_port;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
    }
}
```
