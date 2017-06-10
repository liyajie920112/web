# JS高级

## JS闭包
- 概念: 封闭空间 , 包裹.

- 作用:
    - 延长变量的生命周期.
    - 保护内部数据, 更安全.
    - 为我们提供一种间接(函数)访问函数内部私有数据的方法

- 需求:
    函数内部声明的变量无法在函数外部访问
    但是有时候我们有访问函数内部私有变量的需求.

### 版本一
```js
function f1(){
    var a = 1;
    return function(){
        return a;
    }
}
var b = f1();
console.log(b());// 这样就把 a 打印出来了 1
```

### 版本二
此版本可读可写

```js
function f1(){
    var a = 1;
    return {
        getA:function(){
            return a;
        },
        setA:function(value){
            a = value;
        }
    }
}
var b = f1();
console.log(b.getA());// 1
b.setA(2);
console.log(b.getA());// 2
```

### 版本三

```js
var f = (function(){
    var name = 'liyajie';//私有变量
    return {
        getName:function(){
            return name;
        },
        setName:function(value){
            name = value;
        }
    }
})();
// 上面代码我们使用立即执行函数调用了一下函数, 这样 f 就是返回的对象了.
console.log(f.getName());// liyajie
f.setName('lihaojie');
console.log(f.getName());// lihaojie
```

## js派发一个事件

?> 派发事件有个属性`event._constructed`

```js
function simulateClick() {
  var event = new MouseEvent('click', {
    'view': window,
    'bubbles': true,
    'cancelable': true
  });
  var cb = document.getElementById('checkbox'); 
  var canceled = !cb.dispatchEvent(event);
  if (canceled) {
    // A handler called preventDefault.
    alert("canceled");
  } else {
    // None of the handlers called preventDefault.
    alert("not canceled");
  }
}
```