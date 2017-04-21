## 方式一(全局变量模式)

!> 因为`instance`是全局变量,所以很容易被覆盖

```js
// 定义一个变量用于标记是否已经创建了新对象.
var instance;
// 创建构造函数
function Person(){
    // 如果有了此对象, 直接返回
	if (instance) {
		return instance;
	}
    // 如果没有则将当前对象赋值给instance
	instance = this;
}
var p1 = new Person();
var p2 = new Person();
console.log(p1 == p2);
```

## 方式二(静态属性方式)
```js
function Person(){
    if(Person.instance){
        return Person.instance;
    }
    Person.instance = this;
}
```

## 方式三(惰性函数 + 闭包)
```js
function Person() {
    var instance;
    Person = function() {
        return instance;
    }
    instance = this;
    this.name = 'liyajie';
}
Person.prototype.age = 11;// 这里个Person的原型添加age属性, 其实是给做外层的Person的原型添加的属性.
var p1 = new Person();
var p2 = new Person();
console.log(p1 == p2);//true
console.log(p2.constructor == p1.constructor);//true
console.log(p1.constructor == Person);// false
console.log(p1.age);//11
Person.prototype.sex = '男';// 这里个Person的原型添加sex属性, 是给内部Person的原型添加的属性.
console.log(p1.sex);//undefined)
```

!> 为什么`p1.constructor == Person` 的结果是`false`, 因为在第一次创建对象的时候返回的`instance`是最外层的Person对象,内部又把Person构造函数重写了, 所以`Person`已经不是最外层的`Person`了, 而是内部的`Person`,`p2.constructor == p1.constructor`结果是true, 是因为每次返回的对象都是第一次创建的对象,也就是最外层`Person`创建的对象, 所以 `p1`和`p2`的构造器都是做外层的`Person`.

## 方式四(惰性函数 + 闭包 + 重置构造器)

```js
function Person(){
    var instance;
    Person = function(){
        return instance;
    }
    // 修改Person的原型为当前对象的原型.
    Person.prototype = Object.getPrototypeOf(this);
    // 将当前Person对象复制给instance
    instance = new Person();
    // 修改对象的构造器属性为内部Person
    instance.constructor = Person;
    
    return instance;
}
var p1 = new Person();
var p2 = new Person();
console.log(p1 == p2);//true
console.log(p1.constructor == p2.constructor);//true
console.log(p1.constructor == Person);//true
```