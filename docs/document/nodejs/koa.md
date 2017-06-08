## 使用koa桥接实现跨域

```js
const Koa = require('koa')
const KoaRoute = require('koa-router')

const app = new Koa();
const router = new KoaRoute()
app.use(router.routes())
    .use(router.allowedMethods())

// 创建一个http请求方法
function httpGet(url) {
    var promise = new Promise(function (resolve, reject) {
        const req = https.get(url, function (res) {
            var datas = [];
            var size = 0;
            res.on('data', (d) => {
                datas.push(d)
                size += d.length
            })
            res.on('end', function () {
                var buff = Buffer.concat(datas, size)
                resolve(buff) // 获取到数据后会调用这个方法
            })
        })
        req.end();
    })

    // 当执行resolve的时候就会来这里执行这个方法， 将数据返回
    promise.then(function (data) {
        return data;
    })
    return promise; // 返回Promise对象
}
```

使用上面的方法

```js
router.get('test',async (ctx,next) => {
    var info = await httpGet('url');

    ctx.body = info.toString();
})
```