---
title: 'linux sh: 1: node: Permission denied'
date: 2017-08-22 09:15:20
tags:
  - nodejs
  - node
categories:
  - nodejs
---

### 问题

在 `Linux` 系统下 npm 安装报错 没用权限

```
root@zan:~# npm install spy-debugger -g
npm WARN deprecated express@2.5.11: express 2.x series is deprecated
npm WARN deprecated connect@1.9.2: connect 1.x series is deprecated
/root/.nvm/versions/node/v8.2.1/bin/spy-debugger -> /root/.nvm/versions/node/v8.2.1/lib/node_modules/spy-debugger/lib/index.js

> spy-debugger@3.6.5 postinstall /root/.nvm/versions/node/v8.2.1/lib/node_modules/spy-debugger
> node lib/scripts/postinstall.js

sh: 1: node: Permission denied
npm ERR! file sh
npm ERR! code ELIFECYCLE
npm ERR! errno ENOENT
npm ERR! syscall spawn
npm ERR! spy-debugger@3.6.5 postinstall: `node lib/scripts/postinstall.js`
npm ERR! spawn ENOENT
npm ERR! 
npm ERR! Failed at the spy-debugger@3.6.5 postinstall script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2017-08-22T01_12_38_462Z-debug.log
```

### 解决

执行一下命令即可

```
npm config set unsafe-perm true
```

如果还不行 再执行如下

```
npm config set user 0
npm config set unsafe-perm true
sudo npm install -g sm
```

### 参考

[参考](http://sourcode.net/sh-1-node-permission-denied/)