# ES6新特性

[阮一峰大神教程](http://es6.ruanyifeng.com/)

## Arrow Functions 箭头函数

> 输入ES6

```js
var a = () => {};
var b = (c) => c;

const double = [1,2,3].map((num) => num * 2);
console.log(double);// [2,4,6]

var bbb = {
  _name:'Paul',
  _friends:['Alan','Lynn'],
  printFriends(){
    this._friends.forEach((item) => console.log(this._name + ' knows '+ item));
  }
}
bbb.printFriends();
// "Paul knows Alan"
// "Paul knows Lynn"
```

> 输出ES5

```js
var a = function a() {};
var b = function b(c) {
  return c;
};
var double = [1, 2, 3].map(function (num) {
  return num * 2;
});
console.log(double); // [2,4,6]

var bbb = {
  _name: 'Paul',
  _friends: ['Alan', 'Lynn'],
  printFriends: function printFriends() {
    var _this = this;

    this._friends.forEach(function (item) {
      return console.log(_this._name + ' knows ' + item);
    });
  }
};
bbb.printFriends();
// "Paul knows Alan"
// "Paul knows Lynn"
```

!> 需要注意的是对象中的函数直接使用 `methodName(){}`

## Classes 类

### 基本语法

> 传统方式

```js
function Student(name){
    this.name = name;
}
Student.prototype.showName = function(){
    return 'My name is ' + this.name;
}
var stu = new Student('liyajie');
```

> ES6的方式

```js
// 创建一个类
class Student{
    // 类的构造方法
    constructor(name){
        this.name = name;
    }
    showName(){
        return 'My name is ' + this.name;
    }
}
console.log(typeof Student); // function
Student === Student.prototype.constructor; // true
```

!> ES5中的构造函数Student对应ES6中的构造方法(constructor)

!> 上面代码表明, 类的数据类型就是函数, 类本身就指向构造函数.

构造函数的`prototype`属性, 在ES6中继续使用, 事实上, 类的所有的方法都定义在类的`prototype`属性上面.

```js
class Student{
    constructor(){
        // ...
    }
    toString(){
        // ...
    }
    toValue(){
        // ...
    }
}
// 等同于.
Student.prototype = {
    toString(){},
    toValue(){}
}
```
在类的实例上调用方法, 就是调用的原型上的方法.

```js
class B{}
let b = new B();
b.constructor === B.prototype.constructor;// true
```

ES6中类的内部方法是不可枚举的, 可以通过`Object.keys(obj)`和`Object.getOwnPropertyNames(obj)`来验证
ES5中的内部方法是可以枚举的.
`Object.keys(obj)`是只可以获取到可以枚举的属性.

> 类的属性名可以采用表达式

```js
let methodName = 'showName';
class Student{
    constructor(){
        // ...
    }
    [methodName](){
        // ...
    }
}
```

### constructor方法

> constructor是类默认的方法, 通过new创建实例的时候, 会自动调用该方法. 一个类必须有constructor , 如果没有显示声明, 会默认添加一个空的constructor.

```js
constructor(){}
```

> constructor(){} 默认返回实例对象(this), 完全可以返回另外的对象.

```js
class Foo{
    constructor(){
        return Object.create(null);
    }
}
new Foo() instanceof Foo;// false
```
!> 上面代码中我们在constructor中返回一个全新的对象, 结果导致实例对象不是Foo类的实例.

> 类的构造函数不是用new是没法调用的, 会报错, 而普通的构造函数的一个区别是, ES5中的构造函数不用new也可以调用.

```js
class Foo{
    constructor(){
        console.log('I am constructor');
    }
}
// 这样调用会报错
Foo();// Uncaught TypeError: Class constructor Foo cannot be invoked without 'new'
```

### 类的实例对象

实例属性除非显示的定义在其本身(this), 否则都是定义在原型上(即定义在class上)

```js
class Foo{
  constructor(name){
    this.name = name;
  }
  showname(){
   console.log('hehe'); 
  }
  
}
var f = new Foo();
f.showname();// hehe

f.hasOwnProperty('name');// true
f.hasOwnProperty('showname');// false
f.__proto__.hasOwnProperty('showname');// true
```
上面代码可以看出, name是实例属性, 而showname是原型对象的属性.

### 不存在变量提升
- 类的声明不存在提升

```js
new Foo(); // 报错 ReferenceError
class Foo{}
```

```js
{
    let Foo = class{};
    class Bar extends Foo{

    }
}
```
上面代码不会报错, 因为Bar继承Foo的时候已经提前声明了.

### Class表达式
- 和函数一样, 类也可以通过表达式的方式来定义

```js
const MyClass = class Me{
    getClassName(){
        return Me.name;
    }
}

let cl = new MyClass();
console.log(cl.getClassName());// Me
```

!> 上面的类名是`MyClass`, 不是 Me, Me只是在类的内部使用, 如果内部没有使用到 Me, 则可以省略

```js
const MyClass = class { };
```

> 通过Class表达式可以写出立即执行的Class

```js
let student = new class{
  constructor(name){
    this.name = name;
  }
  showName(){
    console.log(this.name);
  }
}('liyajie');

student.showName();
```
上面代码 student 就是一个立即执行的类的实例.

### 私有方法
- ES6没有提供私有方法,但是我们可以通过变通方法模拟实现.

> 方式一, 命名加以区别

```js
class Student{
    aaa(a){
        this._aaa(a);
    }
    _aaa(b){
        return this.name = b;
    }
}
```
这种方式不保险, 因为外界还是可以通过实例来调用私有方法的.

> 方式二, 将私有方法移除模块, 因为模块内的所有方法都是对外可见的.

```js
class Student{
    setName(name){
        _setName.call(this,name);
    }
}
// 模块外的方法, 不在模块内部, 所以对于模块来说就是私有方法
function _setName(name){
    this.name = name;
}
let stu = new Student();
stu.setName('liyajie');
console.log(stu.name);
stu._setName('lihaojie');// 报错 stu._setName is not a function
```

> 方式三, 利用Symbol, 将私有变量名命名成一个Symbol值

```js
// 声明一个symbol值
const setName = Symbol('setName');
class Student{
    // 共有方法
  setN(n){
    this[setName](n);
  }
  // 私有方法
  [setName](name){
    this.name = name;
  }
}

let stu = new Student();
stu.setN('lihaojie'); 
console.log(stu.name);// lihaojie
stu.setName('lll');//stu.setName is not a function
```

### this的指向
