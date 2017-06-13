# JS面试题

## 笔试题一 ★★★

!> 考点: 对象使用[]访问的时候, 如果key是对象的话, 则会调用`.toString()`被转换成字符串

```js
var o = {};
var a = {};
var b = {name:'liyajie'};

o['a'] = '我是a';
o[a] = '我也是a';
o['b'] = '我是b';
o[b] = '我也是b';

console.log(o.a);   // 我是a
console.log(o.b);   // 我是b
console.log(o[a]);  // 我也是b     把a转换成了字符串 a.toString() == '[object Object]'
console.log(o[b]);  // 我也是b  把b转换成了字符串 b.toString() == '[object Object]'
console.log(o['a']);    // 我是a
console.log(o['b']);    // 我是b
```

## 变量和函数提升一 ★★

```js
var a = 123;
function a(){
    console.log(321);
}
console.log(a);// 结果: 123
```
上面代码等价于
```js
var a;// 变量的声明提升到了作用域顶部
function a(){
    console.log(321);
}
a = 123;
```

## 变量和函数提升二 ★★★★★
> 代码如下:

```js
function Foo() {
    getName = function () { 
        console.log(1); 
    };
    return this;
}
Foo.getName = function () { 
    console.log(2);
};
Foo.prototype.getName = function () { 
    console.log(3);
};
var getName = function () { 
    console.log(4);
};
function getName() { 
    console.log(5);
}

//请写出以下输出结果：
Foo.getName();          // 2
getName();              // 4
Foo().getName();        // 1
getName();              // 1
new Foo.getName();      // 2
new Foo().getName();    // 3
new new Foo().getName();// 3
```
> 解析:

1. Foo.getName();

    1.1 调用的Foo的静态getName()方法, 只有一个静态方法, 所以打印出 2
2. getName();

    2.1 直接调用 getName() 就相当于 window.getName(), 因为getName()是全局作用域中的,全局作用域中只有 4 和 5, 又因为4 是函数表达式声明的函数, 所以 `var getName` 会有变量提升,提升到作用域顶部, 又因为函数也会提升, 所以 5 也会被提升上去, 导致 4 的赋值5 的后面, 所以 打印出 4 
3. Foo().getName();

    3.1 先执行`Foo()`, `Foo()`的返回值再执行`getName()`,`Foo()`执行后返回`this`, 因为`Foo()`是`window`调用的, 所以`this`指向`window`, 因为 上面两行代码执行后, `var getName`提升了声明, 所以执行`Foo().getName()`后`getName`被重新赋值了, 所以打印出 1
4. getName();

    4.1 再次调用`getName()`, 相当于 `window.getName()` 因为执行了上面三行代码, 所以`var getName`最后被重新赋值的是`Foo`构造函数中的代码 , 所以还是打印 1
5. new Foo.getName();

    5.1 `new Foo.getName()`优先级是, 先执行`Foo.getName()`, 再执行`new`, `Foo.getName()`的结果是就是第一行代码执行的结果 2, 前面加了`new`关键字之后, 结果还是一样 结果打印 2
6. new Foo().getName();

    6.1 这行代码先执行`new Foo()`, 再执行`.getName()`, `new Foo()`创建个新的实例对象, `.getName()`就是这个实例对象调用`.getName()`方法, 也就是调用对象的`.getName()`方法, 自身没有`.getName()`方法, `Foo`的原型对象上有 , 所以调用`Foo.prototype.getName`方法, 结果打印 3
7. new new Foo().getName();

    7.1 这行代码先执行`new Foo().getName()`, 再执行`new`, 第6行代码执行结果是3, 所以在前面加了`new` , 结果不变, 还是 3

## 函数形参名称一样的问题 ★★

> 非严格模式下

```js
function sum(a , b, a){
    console.log(a + b + a);
}
sum(1,2,3);// 打印出来的结果是: 8
```
> 解析:

函数的形参相当于函数的局部变量, 所以后面的会把前面的覆盖掉 , 
所以上面函数的 a 是3, 因为 3是传给后面那个 a 的, 把之前的 a 覆盖了, 3 + 2 + 3 = 8


## 第一套

### (1) CSS实现垂直水平居中
CSS实现垂直水平居中, HTML结构如下:
```html
<div class='box'>
    <div class='con'></div>
</div>
```
> 解析

- 方式一: 水平居中对于div来说使用`margin:0 auto;`垂直居中可以使用绝对定位: `top:50%;left:50%;margin-left:-盒子宽度/2;margin-top:-盒子高度/2`
- 方式二: 水平居中对于div来说使用`margin:0 auto;`垂直居中可以使用css3中的变换: `transform:translate(-50%,-50%);`

### (2) 使用原生js或者jQ循环出li的内容
```html
<ul>
    <li>test1</li>
    <li>test2</li>
    <li>test3</li>
    <li>test4</li>
    <li>test5</li>
</ul>
```
> 解析

```js
// 原生js
var lis = document.getElementsByTagName('li');
for(var i = 0, len = lis.length; i < len; i++){
    console.log(lis[i].innerText);
}
// jQuery
$('ul>li').each(function(index,value){
    console.log($(value).text());
});
```

### (3) ajax请求中get和post的区别
- get请求:
    - 会将请求的参数放到url后面
    - 一般用于获取数据
    - 请求是数据长度有限
- post请求:
    - 请求的参数会放在请求头中发送到服务器
    - post可以上传文件

### (4) js中split()和join()的区别.
- split(): 是字符串的方法,使用某个字符将字符串分割, 并返回分割后字符串的数组.
- join() : 是数组的方法, 使用某个字符将数组中的每个元素进行连接, 并返回连接后的字符串.

### (5) 递归查找json对象的问题
方法: getNodeById(data,id);
```js
var json = {
    id:1,
    name:1,
    children:[
        {
            id:2,
            name:2,
            children:false
        },
        {
            id:3,
            name:3,
            children:[
                {
                    id:5,
                    name:5,
                    children:[
                        {
                            id:7,
                            name:7,
                            children:false
                        },
                        {
                            id:8,
                            name:8,
                            children:false
                        },
                        {
                            id:9,
                            name:9,
                            children:false
                        }
                    ]
                }
            ]
        },
        {
            id:4,
            name:4,
            children:[
                {
                    id:6,
                    name:6,
                    children:[
                        {
                            id:10,
                            name:10,
                            children:false
                        }
                    ]
                }
            ]
        }
    ]
}
```
> 解析

```js
// 递归实现
function getNodeById(data,id){
    if(data.id == id){
        return data;
    }
    if(data.children){
        var c = data.children;
        for(var i = 0, len = c.length; i < len; i++){
            var r = getNodeById(c[i],id);
            if(r){ return r; }
        }
    }
}
// 使用
var node = getNodeById(json,8);
console.log(node);
```

## 第二套

### (1) http状态吗及含义
- 100-199 用于指定客户端应相应的某些动作。 
- 200-299 用于表示请求成功。 
- 300-399 用于已经移动的文件并且常被包含在定位头信息中指定新的地址信息。 
- 400-499 用于指出客户端的错误。 
- 500-599 用于支持服务器错误。

[详细信息](http://tool.oschina.net/commons?type=5)

### (2) 设置浮动后, 元素的display的值是什么?
不管什么元素, 只要浮动后`display`就是block

### (3) cookies, sessionStorage和localStorage的区别
- Cookie: 

    - 生命周期: 默认是浏览器关闭就失效, 可以设置失效时间.
    - 存放数据大小: 4K左右
    - 与服务器通信: 每次都会携带在HTTP头中, 如果使用cookie保存过多数据会带来性能问题
    - 易用性: 我们需要自己封装下, 原生的cookie接口不友好.
- sessionStorage:

    - 生命周期: 仅在当前会话中生效, 关闭浏览器或者页面后被清除.
    - 存放数据大小: 一般5M, 可以更多.
    - 与服务器通信: 仅保存在客户端, 不参与与服务器的通信.
    - 易用性: 原生接口可以接受, 也可以再次封装.
- localStorage:

    - 生命周期: 只要不手动清除, 就会永久保存
    - 存放数据大小: 一般5M, 可以更多
    - 与服务器通信: 仅保存在客户端, 不参与与服务器的通信
    - 易用性: 原生接口可以接受, 也可以再次封装.

### (4) 说下下面代码会返回什么
#### 4.1 

```js
var a = 6;
setTimeout(function(){
    console.log(a); // 66
    a = 666;
},1000)
a = 66;
```
> 解析

?>
javascript是单线程的语言, 只有等前面的任务处理完以后才会处理后面的任务, 而且是一个连续同步执行的, 不像多线程可以同时处理多个任务. 
javascript中的任务分为同步任务和异步任务, 同步任务就是主线程一个个排队执行任务, 异步任务则不进入主线程任务, 
而是被加入到任务队列中,任务队列的任务只有等主线程任务执行完成之后才会执行任务队列的任务, setTimeout虽然是异步的, 
仅仅是在1000秒后被加入任务队列并没有执行, 执行的话需要等待前面的主线程的任务和任务队列中前面的任务执行完成之后才会被执行.

#### 4.2
```js
var a = 2;
var f = (function(){
	var a = 3;
	return function(){
		a++;
		alert(a);
	}
})()
f();// 4
f();// 5
// 因为f是一个闭包, 所以a会一致在内存中不会销毁, 所以第一次是4, 
// 第二次因为a没有被销毁, 所以还是4, 4++ 就成了5.
```
### (5) 关于select的操作
- 动态给select添加option
```js
document.getElementById('select').options(new Option('我是显示的文本', 1));
```

### Http缓存与离线缓存

[参考地址](http://www.cnblogs.com/zikai/p/4973355.html)

Http缓存是当发送请求的时候, 发现已经存在缓存版本, 在默认使用缓存版本, 而不是取服务器提取这个文档.
- 好处
    - 减少了冗余数据的传输
    - 减轻了服务器压力
    - 提高了网页的加载速度

### 离线缓存

### 去除数组中重复的元素

- 方式一

```js
var arr = [1,2,3,4,3,3,2,5]
var map = new Map()
arr.forEach((value,index) => {
    map.set(value,index)
})
var newArr = Array.form(map.keys())
console.log(newArr)
```

- 方式二

```js
var arr = [1,2,3,4,3,3,2,5]
var newArr = []
arr.forEach((value,index) => {
    if(newArr.indexOf(value) == -1){
        newArr.push(value)
    }
})
console.log(newArr)
```

### 定义一个方法取解析url, 并返回解析后的对象

```js
function parseUrl(url) {
    var a = document.createElement('a')
    a.href = url;
    return {
        source: url, // 原始url
        hostname: a.hostname, // 主机名
        protocol: a.protocol.replace(':', ''), // 请求协议
        port: a.port || 80, // 端口号
        query: a.search, // 查询参数
        path: a.pathname, // 路径
        hash: a.hash, // 哈希值
        params: (() => { // 将查询参数解析成对象
            var q = a.search.substr(1)
            var kv = q.split('&')
            var obj = {}
            kv.forEach((item, index) => {
                var kvArr = item.split('=')
                var k = kvArr[0]
                var v = kvArr[1]
                obj[k] = v
            })
            return obj
        })()
    }
}
var urlObj = parseUrl('https://www.baidu.com:90/search?name=liyajie&age=12#abc#bbb')
console.log(urlObj)
```

### 使用flex布局实现一个上中下布局

```html
<div class='container'>
    <div class='header'></div>
    <div class='content'></div>
    <div class='footer'></div>
</div>
```

```css
html,body{
    height: 100%;
}
.container{
    display:flex;
    height: 100%;
    flex-direction: column;
}
.container .header,
.container .footer{
    height: 50px;
    width: 100%;
    background-color: #0094ff;
}
.container .content{
    flex: 1;
    width: 100%;
}
```