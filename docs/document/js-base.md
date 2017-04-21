# JS基础

## 进制转换
- 十进制 -> 2进制  11 => (11).toString(2) = 1010
- 十进制 -> 8进制  11 => (11).toString(8) = 013 (0开头说明是8进制) console.log(013);// 11
- 十进制 -> 16进制  11 => (11).toString(16) = b (16进制是的10就从a开始了) console.log(0xb);//11

> 1 2 4 8 方式转换二进制 [http://www.cnblogs.com/mgso/p/6181095.html](http://www.cnblogs.com/mgso/p/6181095.html)

## in 和 hasOwnProperty
> in 和 hasOwnProperty有什么作用? 两者之间有什么区别?


## instanceof 的作用
> 判断该对象的构造函数的原型对象是否在该对象的原型链上, 如果在则返回true, 否则返回false

```js
function Person(){}
var p = new Person();
console.log(p instanceof Person); // true
// 如果我们在这里修改了Person的原型对象
Person.prototype = {};
console.log(p instanceof Person); // false
```

## Object和Function的关系
```js
Object instanceof Function // true
Function instanceof Object //true
对象 instanceof 构造函数
```
```js
1. Function.prototype == Function.__proto__ == Object.__proto__;
2. Function.constructor == Function;
3. Function.prototype.__proto__ == Object.prototype;
4. Object.constructor == Function;
5. Boy.constructor == Function;
6. Boy.__proto__ == Function.__proto__ == Function.prototype;
7. Object.__proto__ == null
```

## with语句的使用
- 作用: 用于扩展作用域链

> 语法: 
```js
with (expression) {
  statement
}
```

> 注意点:
    1. 大括号中的this指向是window
    2. 添加属性需要加前缀
    3. 严格模式下已经禁用
    4. 性能不好,会破坏词法作用域

- expression
    - 将expression添加到作用域链，以便在statement运行时使用。expression 外面需加括号。
- statement
    - 任何语句都行。如果不止一句，则需用block符号({ ... })将其括起來。

- 性能方面利弊
    - 利: with語句可以在不造成性能损失的情況下，减少变量的长度。其造成的附加计算量很少。使用'with'可以减少不必要的指针路径解析运算。需要注意的是，很多情況下，也可以不使用with語句，而是使用一个临时变量來保存指针，来达到同样的效果。
    - 弊: with语句使得程序在查找变量值时，都是先在指定的对象中查找。所以那些本來不是这个对象的属性的变量，查找起來将會很慢。如果是在对性能要求较高的场合，'with'下面的statement语句中的变量，只应该包含这个指定对象的属性。

> 具体用法:

```js
var o = {
    name:'liyajie',
    friends:{
        one:{
            name:'liyajie',
            age: 23
        },
        two:{
            name:'lisong',
            age: 26
        }
    }
}
// 不使用with修改friends的名字和年龄
o.friends.one.name = '我被修改了';
o.friends.one.age = 10;
// 使用with修改name和age
with(o.friends.one){
    name = '修改了';
    age = 11;
}
```
!> with语句中的代码可以修改, 可以删除, 但是不可以在不加前缀的情况下新增属性.

```js
with(o.friends.one){
    name = '修改了';
    age = 11;
    sex = '男';// 不生效
    delete age;// 生效
}
```

## == 比较注意点
- `对象1 == 对象2` 比较的是地址
- `对象1 == 基本值` 内部有隐式转换 (对象1.valueOf == 基本值)

## 基本值访问属性或方法 'demo'.lenfth
- 内部会创建一个对应的对象
- 通过对象访问
- 最后销毁对象

## JS作用域

- 两种: 
    1. script 全局作用域
    2. function 局部作用域
- js没有块级作用域(try...catch 除外)
- 词法作用域
    - 在写完之后就已经确定了作用域

- 动态作用域
    - 在运行的时候确定作用域

- 访问规则
    - 先在自己的作用域中查找, 找不到则去上一级的作用域中查找, 查找到之后开始遍历对象所有的属性, 找到则返回.
    ```js
    var o = { name : 'liyajie'};
    function test(){
        console.log(o.name);
    }
    ```