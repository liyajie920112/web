### 箭头函数

> ES5

```js
var createGreeting = function(message,name){
    return message + name;
}
```

> ES6

```js
var createGreeting = (message,name) => {
    return message + name
}
```

> 体验优点, this指向问题

> ES5

```js
var person = {
    name: 'liyajie',
    handlerMsg: function (msg, handler) {
        handler(msg)
    },
    receive: function () {
        var that = this;
        this.handlerMsg('Hello ', function(msg){
            console.log(msg + that.name)
        })
    }
}
person.receive();
```

> ES6

```js
var person = {
    name: 'liyajie',
    handlerMsg(msg, handler) {
        handler(msg)
    },
    receive() {
        // console.log里的this则指向了person
        this.handlerMsg('Hello ', msg => console.log(msg + this.name))
    }
}
person.receive();
```

### 函数默认值

```js
function greet(msg='hello',name='liyajie'){
    return msg + ' '+ name;
}
console.log(greet());// hello liyajie
console.log(greet('你好'));// 你好 liyajie
```

> 函数作为默认值

```js
function greet(sayhi = () => console.log('test')){
    sayhi()
}
greet('hello')
```

### 解构赋值

```js
let name = 'liyajie'
let age = 24
let person = {
    name,
    age
}
console.log(person) // { name: 'liyajie', age: 24 }

let test = 'test-test'

let team = {person,test}
console.log(team) // { person: { name: 'liyajie', age: 24 }, test: 'test-test' }
```

### 对象增强

```js
var color = 'red'
var speed = 100
function go(){
    console.log('gogogo')
}
var car = {color,speed,go}
console.log(car.color) // red
console.log(car.speed) // 100
car.go() // gogogo
```

> 修改go方法

```js
var car = {
    go(){
        console.log('gogogo')
    }
}
// 等价于
var car = {
    'go'(){
        console.log('gogogo')
    }
}
// 等价于
var car = {
    ['go'](){
        console.log('gogogo')
    }
}
// 等价于
var car = {
    ['go']:function(){
        console.log('gogogo')
    }
}
// 等价于
var car = {
    'go':function(){
        console.log('gogogo')
    }
}
// 等价于
var drive = 'go'
var car = {
    color,
    speed,
    [drive](){
        console.log('gogogo')
    }
}
```

### 扩展运算符 `...`

```js
console.log([1,2,3]) // [1,2,3]
console.log(...[1,2,3]) // 1 2 3
```

```js
let first = [1,2,3]
let second = [4,5,6]
first.push(second) // [1,2,3,[4,5,6]]
first.push(...second) // [1,2,3,4,5,6]
first.push(...second) // [1,2,3,4,5,6,4,5,6]
```

```js
function sum(a,b,c){
    return a + b + c;
}
console.log(sum(...[1,2,3])) // 6
console.log(sum(...[4,5,6])) // 15
```

### 模板字符串

```js
// 普通字符串拼接方式
let str1 = 'one';
let str2 = 'two';
let strSum = str1 + str2 + ' end';

// 使用模板语法
let strSum2 = `${str1} ${str2} end`
```

### 对象的解构赋值

```js
var obj = {
    color:'blue'
}
var {color} = obj
console.log(color) // blue
```

> 来个更牛逼的

```js
var obj = {
    name:'liyajie',
    age:24,
    location:'Beijing'
}
var {name,location} = obj
console.log(name,location) // liyajie Beijing

// 使用别名
var { name : firstName, location: lt} = obj
console.log(firstName,lt) // liyajie Beijing
```

```js
var [first,,,,fifth] = ['one','two','three','four','five'];
console.log(first,fifth) // one five
```

> 实际使用

```js
var person = [{
    name: 'liyajie1',
    age: 23,
    email: '865808403@qq.com1'
}, {
    name: 'liyajie2',
    age: 24,
    email: '865808403@qq.com2'
}, {
    name: 'liyajie3',
    age: 25,
    email: '865808403@qq.com3'
}, {
    name: 'liyajie4',
    age: 26,
    email: '865808403@qq.com4'
}, {
    name: 'liyajie5',
    age: 27,
    email: '865808403@qq.com5'
}]

person.forEach((obj) => console.log(obj))
/*
{ name: 'liyajie1', age: 23, email: '865808403@qq.com1' }
{ name: 'liyajie2', age: 24, email: '865808403@qq.com2' }
{ name: 'liyajie3', age: 25, email: '865808403@qq.com3' }
{ name: 'liyajie4', age: 26, email: '865808403@qq.com4' }
{ name: 'liyajie5', age: 27, email: '865808403@qq.com5' }
*/
```

> 获取每个name属性

```js
person.forEach(({name}) => console.log(name))
/*
liyajie1
liyajie2
liyajie3
liyajie4
liyajie5
*/
```

> 获取第二个的email

```js
let [,Two] = person
function log({email}){
    console.log(email) // 865808403@qq.com2
}
log(Two)
```

### Array.from

- 可以将伪数组转换成真数组

```js
const products = document.getElementsByClassName('product')
console.log(products) // 类型是HTMLCollection
const arrProducts = Array.from(products)
console.log(arrProducts) // 类型是Array
// 这样我们就可以使用数组的一些方法了, 如: filter, forEach等
```

### Promise

```js
// 创建一个Promise对象, 回调函数有两个参数, resolve在成功的时候执行,reject在失败的时候执行
var d = new Promise((resolve, reject) => {
    if (false) {
        resolve('success')
    } else {
        reject('error')
    }
})
// 当调用resolve的时候, 则会过来执行then
d.then(data => console.log('success : ', data))
// 当reject执行的时候, 则会过来执行catch
d.catch(err => console.log('err : ', err))
```

> then和catch的另外的写法

```js
d.then(data => console.log('success :',data),err => console.log('err :' , err))
```

> 链式调用

```js
d.then(data => console.log('success :', data))
    .catch(err => console.log('err :' + err))
```

> 多次then

```js
d.then(data => {
    console.log('success : ',data)
        return 'sss : ' + data;
    })
    .then(data => console.log(data)) // 这里的data就是上面then的返回值
    .catch(err => console.log('err :' + err))
```

> 手动抛出异常的情况

```js
var d = new Promise((resolve,reject) => {
    throw new Error('I am error') // 这样代码后面的代码不会被执行
    if(true){
        resolve('success')
    } else {
        reject('err')
    }
})
d.then(data => console.log('success :',data))
.catch(err => console.log('err :' , err)) // 手动抛出的异常也会走catch, 不会走then
```

> 在then后抛出异常

```js
d.then(data => {
    console.log('success : ' , data)
    throw new Error('I am Error')   -----     // 因为这里抛出了异常, 所以这行之后的代码不会被执行
    return 'I am success';              |
})                                      | 
.then(data => {                         |
    console.log('success2 :' , data)    |
})                                      |
.catch(err => {   <---------------------|
    console.log('err : '+ err)
})
// 打印结果:
//  success : success
//  err :Error: I am Error
```

> 加入延时操作

```js
var p = new Promise((resolve,reject) => {
    setTimeout(() => {
        if(true){
            resolve('success');
        } else {
            reject('error')
        }
    },2000)
})

// 注意: then中的代码只有在resolve这个方法执行之后才能被执行, 否则不会被执行, 
// 所以resolve被延迟执行了, 那么then这个方法也会被延时执行
p.then(data => {
    console.log('success : ' , data)
})
```


### Map对象

字典

- API
    - set() : 设置map 
    - get() : 根据key获取value
    - clear() : 清空map
    - size : map长度属性
    - has() : 判断是否有某个key

```js
// 创建Map对象
let myMap = new Map();
myMap.set('name', 'liyajie')
myMap.set('hello', 'world')

console.log(myMap.get('name'))// liyajie
console.log(myMap.size) // 2
console.log(myMap.has('age')) // false
myMap.clear(); // 清空map
console.log(myMap.size) // 0
```

- Iterators
    - keys()
    - entries()
    - values

```js
let myMap = new Map();
myMap.set('hello','world')
myMap.set('name','liyajie')

for(let key of myMap.keys()){
    console.log(key)
}
// 输出结果是 hello name
for(let value of myMap.values()){
    console.log(value)
}
// 输出结果是 world liyajie
for(let [key,value] of myMap.entries()){
    console.log(key+' = '+value)
}
// 输出结果是 hello = world, name = liyajie
```

### WeakMap

- WeakMap和Map的区别是, WeakMap只能使用对象作为键
- API
    - get()
    - set()
    - has()
    - delete()