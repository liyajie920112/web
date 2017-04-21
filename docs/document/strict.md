# JS严格模式

> [参考地址 MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)

## 开发模式
1. 非严格模式( 默认 )
2. 严格模式( use strict);

### 开启严格模式
1. 字符串命令 'use strict'
2. 位置: 当前作用域顶端

### 兼容性问题
?> 不存在兼容性问题

### 严格模式下使用注意
1. 所有变量必须使用 `var` 声明变量

    1.1 非严格模式 `a = 123;`// 不报错, 可以正常使用

    1.2 严格模式 `a = 123;`// 报错
2. 禁止使用八进制 ( 以0开头的数值会以8进制解析 )

    2.1 非严格模式 console.log(010); // 打印出 8

    2.2 严格模式    console.log(010);// 打印出10, 会当成10进制来解析
03. 禁止使用with
04. 不能删除全局的变量

    4.1 非严格模式 `var a = 111; delete a;`// 不报错, 也删除不了

    4.2 严格模式`var a = 111;delete a;`//直接报错
05. 不能在if语句中声明函数

    5.1 非严格模式, 不报错
    ```js
    if(true){
        function a(){
            console.log('我是在if语句中的函数');
        }
    }
        ```
    5.2 严格模式下 报错

06. 函数形参不能出现重名的情况
    6.1 非严格模式下: `function sum(a,b,a){ console.log(a + b + a); }`//没问题
    
    6.2 严格模式下直接报错
07. 不能使用callee - 函数本身 | caller - 函数调用者
08. 不能使用eval 和 arguments作为函数或变量的名称

    8.1 非严格模式下: `var eval = 1; var arguments = 2;`//这样声明变量不报错
    8.2 严格模式下: 上面这种方式会报错.
09. 修正了this的指向. 

    9.1 非严格模式下: `this`默认指向`window`
    ```js
    function ff(){
        console.log(this);
    }
    ff.call(null);//打印出window
    ```
    9.2 严格模式下: `this` 指向`undefined`
    ```js
    'use strict'
    function ff(){
        console.log(this);
    }
    ff.call(null);// 打印出null
    ```
10. arguments的表现不一致

    10.1 非严格模式下
    ```js
    function ff(a,b) {
        console.log(arguments);//[1,2]
    	a = 2;
        console.log(arguments);//[2,2]
    }
    ff(1,2);
    ```
    10.2 严格模式
    ```js
    'use strict'
    function ff(a,b) {
        console.log(arguments);//[1,2]
    	a = 2;
        console.log(arguments);//[1,2]
    }
    ff(1,2);
    ```
11. 对象中不能出现同名属性

    11.1 非严格模式
    ```js
    var o = {
        name : 'liyajie',
        name : 'paul' // 不会报错
    }
    ```
    11.2 严格模式
    ```js
    'use strict'
    var o = {
        name : 'liyajie',
        name : 'paul' // 报错
    }
    ```