> 什么是对象?

只要拥有属性和方法就是对象

> 什么是面向对象?

面向对象是一种解决问题的方式. 和面向过程不同的是面向过程的关注点是关注解决问题的每一个步骤, 而面向对象则是对象, 
比如洗衣服, 面向过程就会看重洗衣服的每一步, 面向对象则是找个能洗衣服的东西来做这件事情.

> 面向对象的三大特性?

## 封装

提高复用性, 把信息隐藏(用的时候只需要调用封装之后的方法即可, 不需要关心内部实现), 把函数和变量封装到对象内部.

## 多态

同一操作, 作用于不同的对象, 会产生不同的解释和行为.

### 多态在面向对象程序设计中的作用

> 借用Martin Fowler在<<重构: 改善既有代码的设计>>中提到的一句话

多态的最根本的好处在于, 你不必在想对象询问 "你是什么类型" , 而后根据得到的答案调用对象的某个行为--你只管调用该行为就是了, 其他的一切多态机制都会为你安排妥当.

换句话说, 多态最根本的作用就是通过把过程化的条件分支语句转换为对象的多态性, 从而消除这些条件的分支语句.

一段未使用多态的代码
```js
// 创建一个叫声方法
var makeSound = function(animal){
    if(animal instanceof Duck){ // 如果是鸭子
        console.log('嘎嘎嘎');
    } else if(animal instanceof Chicken){ // 如果是鸡
        console.log('咯咯咯');
    }
}
// 鸭子对象
var Duck = function(){};
// 鸡对象
var Chicken = function(){};
makeSound(new Duck()); // 嘎嘎嘎
makeSound(new Chicken()); // 咯咯咯
```
!> 上面代码如果有一条狗的叫声, 那么我们还需要在进行修改分支条件, 判断如果是狗, 则: 汪汪汪.

使用多态改进后的代码
```js
var makeSound = function(animal){
    animal.sound();
}
// 鸭子对象
var Duck = function(){};
Duck.prototype.sound = function(){
    console.log('嘎嘎嘎');
}
// 鸡对象
var Chicken = function(){};
Chicken.prototype.sound = function(){
    console.log('咯咯咯');
}
makeSound(new Duck()); // 嘎嘎嘎
makeSound(new Chicken()); // 咯咯咯
```
把不变的东西隔离出来, 把可变的东西各自进行封装.

> 多态的思想
其实就是把  '做什么'和'谁去做' 分离开来, 要实现这一点归根结底就是消除类型之间的耦合. 如果类型之间的耦合没有被消除, 那么我们在makeSound方法中指定了发出叫声的某个对象是什么类型, 它就不可能被替换成另外一种类型.



## 继承

> 什么是继承?

在js中继承是获得存在对象的已有属性和方法的一种方式.
### 继承一 属性拷贝

> 就是将对象的成员复制一份给需要继承的对象.

```js
// 创建父对象
var superObj = {
    name:'liyajie',
    age:25,
    friends:['小名','小丽','二蛋'],
    showName:function(){
        console.log(this.name);
    }
}

// 创建需要继承的子对象
var subObj = {};
// 开始拷贝属性(使用for...in...循环)
for(var i in superObj){
    subObj[i] = superObj[i];
}
// 这里我们打印下subObj看下
console.log(subObj);
// 打印superObj(父对象)
console.log(superObj);
```
上述代码打印结果是一样的, 但是存在几点问题


!>
属性拷贝继承的问题
如果继承过来的成员是引用类型的话, 
那么这个引用类型的成员在父对象和子对象之间是共享的, 
也就是说修改了之后, 父子对象都会受到影响.

### 继承二 原型式继承

> 原型式继承就是借用构造函数的原型对象实现继承. 即 子构造函数.prototype = 父构造函数.prototype

```js
// 创建父构造函数
function SuperClass(name){
    this.name = name;
    this.showName = function(){
        console.log(this.name);
    }
}
// 设置父构造器的原型对象
SuperClass.prototype.age = 25;
SuperClass.prototype.friends = ['小名','小丽'];
SuperClass.prototype.showAge = function(){
    console.log(this.age);
}

// 创建子构造函数, 刚开始没有任何成员
function SubClass(){
}
// 设置子构造器的原型对象实现继承
SubClass.prototype = SuperClass.prototype;
// 因为子构造函数的原型被覆盖了, 所以现在子构造函数的原型的构造器属性已经不在指向SubClass,
// 而是SuperClass,我们需要修正
console.log(SubClass.prototype.constructor == SuperClass);// true
console.log(SuperClass.prototype.constructor == SuperClass);// true
SuperClass.prototype.constructor = SubClass;
// 上面这行代码之后, 就实现了继承
var child = new SubClass();
console.log(child.age);// 25
console.log(child.friends);// ['小名','小丽']
child.showAge();// 25
```
!> 只能继承父构造函数的原型对象上的成员, 不能继承父构造函数的实例对象的成员, 同时父构造函数的原型对象和子构造函数的原型对象上的成员有共享问题

### 继承三 原型链继承

> 原型链继承 即 子构造函数.prototype = new 父构造函数();

```js
// 创建父构造函数
function SuperClass(){
    this.name = 'liyajie';
    this.age = 25;
    this.showName = function(){
        console.log(this.name);
    }
}
// 设置父构造函数的原型
SuperClass.prototype.friends = ['小名', '小强'];
SuperClass.prototype.showAge = function(){
    console.log(this.age);
}
// 创建子构造函数
function SubClass(){

}
// 实现继承
SubClass.prototype = new SuperClass();
// 修改子构造函数的原型的构造器属性
SubClass.prototype.constructor = SubClass;

var child = new SubClass();
console.log(child.name); // liyajie
console.log(child.age);// 25
child.showName();// liyajie
child.showAge();// 25
console.log(child.friends); // ['小名','小强']

// 当我们改变friends的时候, 父构造函数的原型对象的也会变化
child.friends.push('小王八');
console.log(child.friends);["小名", "小强", "小王八"]
var father = new SuperClass();
console.log(father.friends);["小名", "小强", "小王八"]
```
!> 问题是不能给父构造函数传递参数, 父子构造函数的原型对象之间有共享问题.

### 继承四 借用构造函数

> 使用`call`和`apply`借用其他构造函数的成员, 可以解决给父构造函数传递参数的问题, 但是获取不到父构造函数原型上的成员.也不存在共享问题.

```js
// 创建父构造函数
function Person(name){
    this.name = name;
    this.friends = ['小王','小强'];
    this.showName = function(){
        console.log(this.name);
    }
}
// 创建子构造函数
function Student(name){
    // 使用call借用Person的构造函数
    Person.call(this,name);
}
// 测试是否有了Person的成员
var stu = new Student('liyajie');
stu.showName(); // liyajie
console.log(stu.friends); // ['小王','小强']
```

### 继承五 组合继承
> 借用构造函数 + 原型式继承

```js
// 创建父构造函数
function Person(name,age){
    this.name = name;
    this.age = age;
    this.showName = function(){
        console.log(this.name);
    }
}
// 设置父构造函数的原型对象
Person.prototype.showAge = function(){
    console.log(this.age);
}
// 创建子构造函数
function Student(name){
    Person.call(this,name);
}
// 设置继承
Student.prototype = Person.prototype;
Student.prototype.constructor = Student;
// 上面代码解决了 父构造函数的属性继承到了子构造函数的实例对象上了, 
// 并且继承了父构造函数原型对象上的成员, 
// 解决了给父构造函数传递参数问题
// 但是还有共享的问题
```
!> 问题: 还有共享的问题

### 继承六 借用构造函数 + 深拷贝
[前往深拷贝](/object-oriented#深拷贝)
```js
function Person(name,age){
    this.name = name;
    this.age = age;
    this.showName = function(){
        console.log(this.name);
    }
}
Person.prototype.friends = ['小王','小强','小王八'];

function Student(name,25){
    // 借用构造函数(Person)
    Person.call(this,name,25);
}
// 使用深拷贝实现继承
deepCopy(Student.prototype,Person.prototype);
Student.prototype.constructor = Student;
// 这样就将Person的原型对象上的成员拷贝到了Student的原型上了, 这种方式没有属性共享的问题.
```
?> 推荐使用这种方式.

#### 深拷贝

> 使用递归实现, 主要是为了解决对象中引用类型的成员的共享问题.

好处是不管是值类型的属性还是引用类型的成员都不会有共享问题.
```js
// 将obj2的成员拷贝到obj1中, 只拷贝实例成员
function deepCopy(obj1, obj2) {
    for (var key in obj2) {
        // 判断是否是obj2上的实例成员
        if (obj2.hasOwnProperty(key)) {
            // 判断是否是引用类型的成员变量
            if (typeof obj2[key] == 'object') {
                obj1[key] = Array.isArray(obj2[key]) ? [] : {};
                deepCopy(obj1[key], obj2[key]);
            } else {
                obj1[key] = obj2[key];
            }
        }
    }
}

var person = {
    name: 'liyajie',
    age: 25,
    showName: function() {
        console.log(this.name);
    },
    friends: [1, 2, 3, 4],
    family: {
        father: 'ligang',
        mather: 'sizhongzhen',
        wife: 'dan',
        baby: 'weijun'
    }
}
var student = {};
// 将person的成员拷贝到student对象上.
deepCopy(student, person);
```

#### Array.isArray()

> 此方法主要是来判断某个对象是否是数组, 因为是ES5的新特性, 所有有兼容性问题.

```js
// 检查是否是数组对象
function checkIsArray(obj){
    if(Array.isArray){// 如果有这个属性
        return Array.isArray(obj);
    } else {
        return Object.prototype.toString.call(obj) == '[object Array]';
    }
}
```

#### instanceof 
> 简单来说用来判断某个对象是否是由某个构造函数创建的.
> 严谨一点: 用来检查某个对象的构造函数的原型对象是否在当前对象的原型链上, 因为原型链可以任意由我们修改

示例代码如下:
```js
function Person(){}
var person = new Person();
console.log(person instanceof Person); // true
Person.prototype = {};
console.log(person instanceof Person); // false
```