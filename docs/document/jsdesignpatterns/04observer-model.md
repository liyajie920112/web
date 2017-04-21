## 简单版
- 只有简单的订阅和发布功能

!> 缺点: 发布者将订阅者没有订阅的事件也通知了订阅者(观察者)

```js
var demo = demo || {};
demo.clientList = demo.clientList || [];
(function(demo){
    // 订阅事件
    demo.listen = function(fn){
        this.clientList.push(fn);
    }
    // 发布事件
    demo.publish = function(){
        // 发布事件的时候通知订阅的用户
        // for(var key in this.clientList){
        //    this.clientList[key].apply(this,arguments);
        // }
        for(var i = 0, fn; fn = this.clientList[i++];){
            fn.apply(this.arguments);
        }
    }
})(demo)
// 订阅一个
demo.listen(function(a,b,c){
    console.log('我是订阅一号:' + a + b + c);
})
// 再次订阅一个
demo.listen(function(a,b,c){
    console.log('我是订阅二号:' + a + b + c);
})
// 发布
demo.publish(1,2,3);// 发布一次全都通知了.
```

## 改进版一

- 此版本增加了只订阅自己想要订阅的东西, 解决了上一版本的这个问题

!> 缺点: 每个订阅者在发布者处订阅事件的时候,如果有多个发布者, 每一个发布者对象都要编写部分重复的代码.

```js
var demo = demo || {};
demo.clientList = demo.clientList || {};
(function(demo){
    // 订阅事件
    demo.listen = function(key,fn){
        if(!this.clientList[key]){
            this.clientList[key] = [];
        }
        // 将订阅的事件添加到订阅列表中
        this.clientList[key].push(fn);
    }
    // 发布
    /*
    * 改进之后, 这个发布方法的第一个参数就是当时订阅的key, 
    * 这样才能订阅自己喜欢的东西, 所以 arguments的第一个参数就是key
    */
    demo.publish = function(){
        // 借用数组的slice方法
        var arr = [].slice.apply(arguments);
        var key = arr.shift();// 删除第一个, 只需要后面的参数, 返回的第一个参数就是key
        var fns = this.clientList[key];
        if(!fns || fns.length == 0){
            return false;
        }
        for(var i = 0, fn; fn = fns[i++];){
            fn.apply(this,arr);
        }
    }
})(demo);

// 缺点是下面的代码, 每次定义和发布有部分重复代码

// 订阅一个事件 名称: one
demo.listen('one',function(a,b,c){
    console.log('one : ' + a + b + c);
})
// 在订阅一个 名称 : two
demo.listen('two' , function(a,b){
    console.log('two : ' + a + b)
})
// 发布one
demo.publish('two',1,2);// 只打印出 订阅的 two
demo.publish('one',1,2,3);// 只打印出 订阅的 one
```

## 改进版二
- 需要解决的问题: 
    - 多个发布者的时候, 代码大量重复的问题
- 解决思路:
    - 将重复的代码进行封装, 谁要成为发布者就直接将封装