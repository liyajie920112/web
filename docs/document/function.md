# 函数相关 Function

## 创建函数的三种方式

### 方式一

> 直接声明

```js
// 创建一个函数,返回两个数的和
function sum(a,b){
    return a + b;
}
sum(1,2);
```

### 方式二

> 函数表达式

```js
var sum = function(a,b){
    return a + b;
}
sum(1,2);
```

### 方式三

> 使用构造函数创建

```js
// a 和 b 是函数的形参, return a + b; 是函数体
var sum = new Function('a','b','return a + b');
// 调用函数
sum(1,2);
```
!> 使用构造函数的方式需要注意, 最后一个参数是函数体, 前面的是函数的形参

## 函数的相关属性

### length

> `length` 表示函数的期望参数个数(长度), 也就是形参的个数

```js
function Person(){}
console.log(Person.length);// 0

function Person2(name,age){}
console.log(Person2.length);// 2
```

### arguments

> `arguments` 是函数的传进去的实参的数组, 每一个元素都是函数的形参.

```js
function animal(name,color){
    console.log(arguments);
    console.log('这个动物叫: ' + name + ' 颜色是: '+color);
}
// 调用animal
animal('小猫咪', '棕色','男');
/*
 * 先打印: ['小猫咪','棕色','男']
 * 再打印: 这个动物叫: 小猫咪 颜色是: 棕色
 */
```

### 惰性函数

```js
function foo(){
    // 执行一次的代码写在这里
    // ...
    // 执行完成之后覆盖掉foo函数
    foo = function(){

    }
}
```
一般用于初始化

### 即时函数初始化

```js
({
    name:'liyajie',
    init:function(){
        // 这里进行初始化
    }
}).init();
```