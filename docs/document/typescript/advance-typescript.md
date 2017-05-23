## 类型别名
> 语法: `type 类型别名 = 类型`

?> 使用场景 : 常用于联合类型

```ts
type Str = string;
type StrFun = () => Str;
type StrType = Str | StrFun;
function getStr(v : StrType) : Str{
    if(typeof v === 'string'){
        return v;
    } else {
        return v();
    }
}
// exec
console.log(getStr('123'));// 123
console.log(getStr(e => {
    return 'lihaojie';
}));// lihaojie
```

## 字符串字面量类型

```ts
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele:Element,event:EventNames){
    //  do something ...
}
handleEvent(document.getElementById('box'),'click');// 正确
handleEvent(document.getElementById('box'),'dblclick');// 错误: 因为dblclick不在EventNames中
```

## 元组
    - 类似ES6中的解构赋值

> 语法 : `let person:[string,number] = ['liyajie',23]`

```ts
let person : [string,number] = ['liyajie',25];
// 获取元素
console.log(person[0]);// liyajie
console.log(person[1]);// 25
// 添加 元素
person.push('www.baidu.com');
// 添加的元素的类型默认是元组中每个类型的联合属性 string | number
console.log(person[3]);// www.baidu.com

person.push(false); // 报错: 因为false不是元组中类型的联合属性
```

## 枚举

```typescript
enum Days {
    Sun,
    Mon,
    Tue,
    Wed,
    Thu,
    Fir,
    Sat
};
// 等价于
enum Days {
    Sun = 0,
    Mon = 1,
    Tue = 2,
    Wed = 3,
    Thu = 4,
    Fir = 5,
    Sat = 6
};
console.log(Days.Mon); // 1
console.log(Days['Sat']); // 6
console.log(Days[0]); // Sun
```
解析成js后

```js
var Days;
(function (Days) {
    Days[Days["Sun"] = 0] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fir"] = 5] = "Fir";
    Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));
```

### 手动赋值

```ts
enum Days {
    Sun = 7,
    Mon = 6,
    Tue,
    Wed,
    Thu,
    Fir,
    Sat = <any>'sss'
}
console.log(Days[7]);// Tue 以最后的为准
console.log(Days.Sat);// sss
console.log(Days['sss']);// Sat
```
上面这种手动赋值的方式被解析成了
```js
var Days;
(function (Days) {
    Days[Days["Sun"] = 7] = "Sun";
    Days[Days["Mon"] = 6] = "Mon";
    Days[Days["Tue"] = 7] = "Tue";
    Days[Days["Wed"] = 8] = "Wed";
    Days[Days["Thu"] = 9] = "Thu";
    Days[Days["Fir"] = 10] = "Fir";
    Days[Days["Sat"] = 'sss'] = "Sat";
})(Days || (Days = {}));
```

### 计算所得项

```ts
enum Colors {
    Red,
    Green,
    Blue = 'blue'.length // 计算所得项, 将计算的结果赋给Blue
}
console.log(Colors.Blue); // 4
console.log(Colors[4]);// Blue
```

### 常数枚举

```ts
const enum Directions {
    Up,
    Down,
    Left,
    Right
}
```
解析结果
```js
[0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

!> 如果常数枚举中包含计算项, 则会在编译的时候报错.

## 类

### ES6中的类

#### 属性和方法

```js
class Animal {
    constructor(name) {
        this.name = name;
    }

    sayHi(){
        console.log(`My name is ${this.name}`);
    }
}

// exec
const animal = new Animal('小名'); // 默认会调用constructor构造方法
console.log(animal.sayHi()); // My name is 小名
```

#### 类的继承

```js
class Animal {
    constructor (name) {
        this.name = name;
    }

    sayHi(){
        return `My name is ${this.name}`;
    }
}

class Dog extends Animal {
    constructor(name){
        super(name);
        console.log(`Dog name is ${name}`);
    }
    sayHi(){
        console.log('super say hi ' + super.sayHi());
    }
}

const dog = new Dog('小明');
dog.sayHi();
```

#### 存取器

```js
class Person {
    constructor(name) {
        this.name = name;
    }

    get name(){
        return 'liyajie'; // 一直返回 'liyajie'
    }

    set name(v){
        console.log(v);
    }
}

const person = new Person('lihaojie');
person.name = 'wangzi';
console.log(person.name);// liyajie, 因为我们改了get方法, 一直返回 'liyajie'
```

#### 静态方法

```js
class Animal {
    static isAnimal(a){
        return a instanceof Animal;
    }
}
var a  = new Animal();
console.log(Animal.isAnimal(a)); // true : instanceof 是检测a 是否在Animal这个构造函数的原型链上
console.log(a.isAnimal(a)); // a.isAnimal not a function
```

### TypeScript中的类

#### public , private, protected

```ts
class Animal {
    public name; // 声明一个公有属性 name
    public constructor (name) { // 在构造函数中初始化 name 属性
        this.name = name;
    }
}

let animal = new Animal('liyajie');
console.log(animal.name); // liyajie
animal.name = 'lihaojie';
console.log(animal.name);// lihaojie
```

上面这种方式, name是public, 有时候我们不想让外界直接能改掉内部属性, 这个时候我们可以使用private

```ts
class Animal {
    private name ;
    public constructor(name) {
        this.name = name;
    }
}

let animal = new Animal('liyajie');
console.log(animal.name); // liyajie  这里会有语法检测错误
animal.name = 'lihaojie'; // 这里也有语法错误, 因为name是private, 只能在类内部访问使用
```

私有属性是不能在子类中访问的

```ts
class Animal {
    private name;
    public constructor(name) {
        this.name = name;
    }
}

class Dog extends Animal {
    constructor(name) {
        super(name);
        console.log(this.name);
    }
}
let dog = new Dog('xiaoming'); // 报错, 错误如下
```

!> Property 'name' is private and only accessible within class 'Animal'.
5:34:48 PM - Compilation complete. Watching for file changes.

?> 如果我们把`private`改成`protected`就不会有语法错误

[参考自https://ts.xcatliu.com](https://ts.xcatliu.com)