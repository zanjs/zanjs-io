---
title: nginx 代理 nodejs 的 websocket 服务
date: 2018-03-14 11:32:23
tags:
  - nodejs
  - websocket
  - nginx
categories:
  - nginx
---

在使用 `nodejs` 做  `websocket` 服务的时候， 用了 `nginx` 做代理, 发现 浏览器报错了

```
websocket.js:111 WebSocket connection to 'wss://api.gg/socket.io/?EIO=3&transport=websocket&sid=Wa-r7QOs37OgwtToAAAD' failed: Error during WebSocket handshake: Unexpected response code: 400
```

解决方案是  修改 `nginx` 配置 文件


```
location / {
  proxy_pass http://localhost:3001;
  proxy_set_header host $host;
  proxy_set_header X-Forwarded-For $remote_addr;
  proxy_pass_request_headers      on;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "upgrade";
  client_max_body_size  2048m;
}
```

主要是 设置 `proxy_set_header Upgrade $http_upgrade;`  和 `proxy_set_header Connection "upgrade";`

至此完成了