## 创建一个简单的服务

```js
var http = require('http')
http.createServer(function(req,res){
    res.end('Hello Node')
}).listen(1234,'localhost',function(){
    console.log('localhost:1234 启动成功')
})
```

## 创建一个有响应头的服务

```js
var http = require('http')
http.createServer(function(req,res){
    res.writeHead(200,{
        'Content-Type':'text/plain;charset=utf8'
    })
    res.end('Hello Node')
}).listen(1234,'localhost',function(){
    console.log('服务已经启动')
})
```

## fs模块

- 用于操作文件的模块
- API
    - 读取文件
    - 读取文件夹
    - 等...

## url模块
- 主要方法
    - url.parse('url地址')