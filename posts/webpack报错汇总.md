---
title: webpack报错汇总
tags: webpack
---

#### 注: 出现以下报错信息可能不只是下面所列出的原因

##### 端口被占用

* npm run dev 报错信息

```Javascript
    events.js:160
      throw er; // Unhandled 'error' event
      ^

    Error: listen EADDRINUSE 127.0.0.1:9000
        at Object.exports._errnoException (util.js:1022:11)
        ...
```




