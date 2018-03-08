# 原型链

![](/images/prototype-img.jpeg)

```js
function Person(){}
function Boy(){}
Boy.prototype = new Person();
```

> 原型链中的关系
```js
1. Function.prototype == Function.__proto__ == Object.__proto__;
2. Function.constructor == Function;
3. Function.prototype.__proto__ == Object.prototype;
4. Object.constructor == Function;
5. Boy.constructor == Function;
6. Boy.__proto__ == Function.__proto__ == Function.prototype;
7. Object.__proto__ == null
```