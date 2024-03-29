# dynamic-http-proxy
> Koa2 或 Egg.js 请求转发中间件。支持动态转发。可用于微服务内部API代理
## 用法

**初始化中间件和动态代理**
> option说明。
> host ,代理指向的目标机器(域名、ip、端口)，可为空。
> map ,可指定动态代理的函数。返回一个 path 或者 Promise

目标主机全局设定
``` javascript
const httpProxy = require('dynamic-http-proxy')

// 动态逻辑返回求被代理到哪个具体path上
const map = function(path, ctx){
    const newPath='/yourNewPath';
    // Your code
    // You can Dynamic mapping path->target with Redis or Memory
    if (ctx.user.type === 'vip') {
        return '/vipPath'
    }
    return newPath
}

// or an asynchronous function

const map = async function(path, ctx){
    const newPath='/yourNewPath';
    // Your code
    // You can Dynamic mapping path->target with Redis or Memory
    newPath = await Redis.mget(yourQuery)
    return newPath
}

router.post('/proxy/*',  httpProxy({
	host: 'http://target_ip:port',
	map
}))

```

目标主机不设定
``` javascript
const httpProxy = require('dynamic-http-proxy')

// 动态逻辑返回求被代理到哪个具体path上
const map = function(path, ctx){
    if (path === '/proxy/login') {
       return 'http://127.0.0.1:3000/login'
    }
    if (path === '/proxy/product') {
       return 'http://127.0.0.1:3001/product'
    }
    // Your code
    // You can Dynamic mapping path->target with Redis or Memory
    return 'yourpath'
}

router.post('/proxy/*',  httpProxy({
	map
}))

```

