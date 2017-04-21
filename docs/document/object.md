# Object对象的相关属性和方法

## Object原型的方法

### `Object.prototype.constructor`
- 作用: Object的原型对象的构造器属性, 可以通过此属性获取原型的构造器

```js
function Person(){
    this.name = 'liyajie';
}
console.log(Person.prototype.constructor);// Person
console.log(Person.prototype.constructor == Person);// true
```

### `Object.prototype.hasOwnProperty()`
- 作用: 判断某成员是否属于某个对象. 只能判断实例对象.

```js
function Person(){
    this.name = 'liyajie';
}
Person.prototype.age = 25;
var p = new Person();
console.log(p.hasOwnProperty('name'));// true, 因为name是Person的实例对象属性
console.log(p.hasOwnProperty('age'));// false, age是Person构造函数原型的对象
```

### `Object.prototype.isPrototypeOf()`
- 作用: 判断某个对象是否是另一个对象的原型对象

代码说明: 创建一个Dog实例, 检查Dog.prototype是否在Dog实例的原型链上.
[详细说明点击这里](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/isPrototypeOf)
```js
function Dog(){
}
var dog = new Dog();
function Cat(){
}
var cat = new Cat();
// 判断某个对象是否是另一个对象的原型对象
console.log(Dog.prototype.isPrototypeOf(dog));// true, dog
console.log(Dog.prototype.isPrototypeOf(cat));// false
```

### `Object.prototype.propertyIsEnumerable()`
> 判断某对象的成员是否可以遍历出来

```js
var o = {
	name:'liyajie'
};
Object.defineProperty(o,'age',{
	enumerable:false,
	value:12
})
console.log(o);
console.log(o.propertyIsEnumerable('name'));// true
console.log(o.propertyIsEnumerable('age'));// false, 因为enumerable = false
```

### `Object.prototype.toLocaleString()`
- 说明: 方法返回一个该对象的字符串表示。该方法主要用于被本地化相关对象覆盖。

> 大部分时候和调用`toString()`方法结果一样

### `Object.prototype.toString()`

- 说明: 
```js
var o = new Object();
o.toString();           // 返回了[object Object] 第一个object表示类型, 第二个Object表示构造函数
```

### `Object.prototype.valueOf()`
- 作用: 返回基本类型的值

> 语法: `对象.valueOf()`

```js
var str = 'abc';
console.log(str.valueOf());//abc
console.log(typeof str.valueOf());//string

var num = 123;
console.log(num.valueOf());// 123
console.log(typeof num.valueOf());// number
```

## Object的静态方法

### `Object.apply()`
- 作用: 借用其他对象的方法

> 语法: `Object.apply(this,[参数一, 参数二, 参数三,...])`

### `Object.call()`
- 作用: 借用其他对象的方法

> 语法: `Object.call(this,参数一, 参数二, 参数三,... )`

注意:
```js
function test(){
    console.log(this);
}
test.call();// this -> window
var o = {};
test.call(o);// this -> o
```

!> 严格模式下 this的指向已经从 window 变成了 undefined

### `Object.assign()`
- 作用: 拷贝属性, 如果要拷贝多个对象, 则一次传入

> 语法: `Object.assign(对象1, 对象2,...)`

```js
var o1 = {name : 'liyajie'};
var o2 = {age : 25};
var obj = Object.assign(o1,o2);
console.log(obj);// {name:'liyajie',age:25}
```
### `Object.bind`

### `Object.callee` 
- 说明: 函数本身

!> 严格模式已经禁用此方法

> 示例代码:
该代码用来计算1~n的加和
```js
var sum = (function(n){
    if(n == 1){
        return 1;
    }
    return n + arguments.callee(n-1);
})(10)
console.log(sum); // 55
// 如果我们打印arguments.callee 结果是:
/*
function (n){
    if(n == 1){
        console.log(arguments.callee);
        return 1;
    }
    return n + arguments.callee(n - 1);
}
*/
```

### `Object.caller`
- 说明: 指的是函数的调用者

!>严格模式已经禁用此方法

!>当window来调用的时候, caller是null

> 示例代码:
```js
function f1(){
    console.log(f1.caller);// 打印出调用f1的调用者
}
function f2(){
    f1();
}
f2();
// 打印结果是:
// function f2(){
//     f1();
// }
// 说明是f2调用的f1
```

### `Object.create()`
- 作用: 创建一个对象, 并设置创建出来的对象的原型对象

> 语法: `Object.create(要继承的对象)`

```js
var o = {name : 'liyajie'};
var stu = Object.create(o);
// 创建了一个stu对象, 并将o赋值给了stu的原型对象.
```

### `Object.getOwnPropertyDescriptor`
- 作用: 获取属性的描述信息( 描述对象 )
- 属性的描述信息:
    - 该属性对应的值.
    - 该属性是否是可枚举的.
    - 该属性是否是可配置的.
    - 该属性是否是可重写的.

### `Object.defineProperty(对象,属性名, 描述信息)`
- 作用: 给对象添加一个属性

> 语法:
    - confirurable: 是否可以删除和修改该属性
    - writable: 该属性是否可以被重写
    - enumerable: 该属性是否可以被遍历出来 for...in...
    - value: 该属性的值
```js
var o = {};
// 目前o对象是一个空对象
// 给o对象添加一个name属性
Object.defineProperty(o,'name',{
    confirurable:true,// 默认值:true,
    writable:true, // 默认值: true,
    enumerable:true, // 默认值: true
    value:'liyajie' // 属性值
})
```

### `Object.getOwnPropertyNames()`
- 作用: `获取所有示例属性名( key ), 返回值是数组`, 不会获取对象构造函数的原型对象上的成员.
- 和`Object.keys(对象)`的区别是:
    - `Object.keys()` : 只能获取属性enumerable = true的对象的属性,可以枚举的属性
    - `Object.getOwnPropertyNames()`: 属性不可枚举也可以获取到
    - 两者都获取不到原型上的成员的key

> 语法: `Object.getOwnPropertyNames(对象)`

```js
function Person(){
	this.name = 'liyajie';
	this.age = 23;
	this.showName = function(){},
	this.friends = [];
}
// 构造函数的原型上的属性不会获取到
Person.prototype.sex = '男';
console.log(Object.getOwnPropertyNames(new Person()));//["name", "age", "showName", "friends"]
```

### `Object.keys()`
- 作用: `获取所有的key`

!> 注意:`Object.getOwnPropertyNames()`和`Object.keys()` 获取的属性都不包含原型对象上的属性.

```text
- 和`Object.keys(对象)`的区别是:
    - `Object.keys()` : 只能获取属性enumerable = true的对象的属性,可以枚举的属性
    - `Object.getOwnPropertyNames()`: 属性不可枚举也可以获取到
    - 两者都获取不到原型上的成员的key
```

### `Object.preventExtensions(对象)`
- 作用: 使对象扩展新属性, 不可以新增属性

```js
var o = {
    name:'liyajie',
    age:12
}
// 禁用对象扩展 不能添加
Object.preventExtensions(o);
o.sex = '男';	//不生效
o.name='lihaojie';// 生效
delete o.age;	// 生效
console.log(o);
```
!> 严格模式下 不生效的语句会报错 `o.sex = '男';`

### `Object.seal(对象)`
- 作用: 密封对象, 使对象 不可新增属性 + 不可删除属性

```js
var o = {
    name:'liyajie',
    age:12
}
Object.seal(o);
o.sex = '男';// 不生效
o.name = 'li';//生效
delete o.age;//不生效
console.log(o)
```

!> 严格模式下 不生效的语句会报错 `o.sex = '男';delete o.age;`

### `Object.freeze(对象)`
- 作用: 冻结对象, 使对象不可新增属性 , 不可删除属性 , 不可修改属性

```js
var o = {
    name:'liyajie',
    age:12
}
Object.freeze(o);
o.sex = '男';	// 不生效
o.name = 'lili';//不生效
delete o.age;	// 不生效
console.log(o);
```

!> 非严格模式下 修改, 删除, 新增 虽然不成功, 但是不会报错, 在严格模式下`o.sex='男';o.name='lili';delete o.age;`会报错