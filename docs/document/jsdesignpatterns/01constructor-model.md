## 简单的构造函数模式

```js
// 定义构造函数
function Dog(name,age){
    this.name = name;
    this.age = age;
    this.showName = function(){
        return '这只狗叫 : ' + this.name;
    }
}
// 使用
// 创建一个狗对象
var dog1 = new Dog('小明',2);
console.log(dog1.showName());
var dog2 = new Dog('小丽',3);
console.log(dog2.showName());

console.log(dog1.showName == dog2.showName);//false
```
!> 缺点是在每次创建狗的时候, 狗的`showName`方法都是被重新定义的.

## 改进版一
```js
function Dog(name,age){
    this.name = name;
    this.age = age;
    this.showName = sn;
}
function sn(){
    return '这只狗叫 : ' + this.name;
}
var dog1 = new Dog('小明',2);
console.log(dog1.showName());
var dog2 = new Dog('小丽',3);
console.log(dog2.showName());

console.log(dog1.showName == dog2.showName);//true
```

## 改进版二(使用原型)

```js
function Dog(name,age){
    this.name = name;
    this.age = age;
}
Dog.prototype.showName = function(){
    return '这只狗叫 : '+this.name;
}
var dog1 = new Dog('小明',2);
console.log(dog1.showName());
var dog2 = new Dog('小丽',3);
console.log(dog2.showName());

console.log(dog1.showName == dog2.showName);//true
```

## 改进版三(强制使用new)
```js
function Dog(name,age){
    // 判断当前this是否指向Dog
    if(!(this instanceof Dog)){
        return new Dog(name,age);
    }
    this.name = name;
    this.age = age;
}
Dog.prototype.showName = function(){
    return '这只狗叫 : '+this.name;
}
var dog1 = new Dog('小明',2);
console.log(dog1.showName());
var dog2 = Dog('小丽',3);// 不实使用new也可以
console.log(dog2.showName());

console.log(dog1.showName == dog2.showName);//true
```