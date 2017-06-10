## Angular

Angular是一个框架, 为了弥补原生html标签的不足

## Angular指令

指令就是给原生的html标签添加一些自定义属性, 达到自己想要的效果.

- ng-init
- ng-app
- ng-controller
- ng-hide
- ng-if
- ng-show
- ng-repeat
- ng-class
- ng-src
    - 之所以使用ng-src, 因为如果我们直接使用src的话, 当html加载的时候就会取请求src后面的地址, 但是如果地址是插值语法的话, 则会报错, 找不到图片, 所以才会有ng-src属性.
- ng-include
    - 使用Ajax获取到指定的模板内容, 插入到标签中
- ng-transclude
    - 自定义标签指令中, 我们可以使用ng-transclude指定指令模板中的哪部分包含,哪部分不包含, 需要在自定义指令中使用`transclude:true|false`属性进行控制

## Angular控制器

创建一个控制器
```js
// 创建模块
var app = angular.module('app',[]);
// 创建控制器
app.controller('MainController',['$scope',function($scope){
    // 注意: 控制器也是绑定到$scope上的

    this.name = 'liyajie'; // 在控制器的model上绑定一个name, this就是当前控制器
    $scope.name = 'angular'; // 在$scope上绑定一个name
    // ...
}])
```
使用
```html
<div ng-app='app' ng-controller='MainController as mc'>
    <!--也就是将mc绑定到$scope-->

    <!--使用控制器上的name-->
    <h1>{{ mc.name }}</h1>
    <!--使用$scope上的name-->
    <h2>{{ name }}</h2>
</div>
```

## Angular过滤器

- 日期过滤器
    - `date:'yyyy/MM/dd HH:mm:ss'`
- 转大写
    - uppercase
- 取前多少位
    - limitTo: 8
- 排序
    - orderBy: '-price'
- 显示货币符号
    - currency
- 保留几位小数
    - number : 2


## 自定义指令

```js
var app = angular.module('app',[]);
app.directive('myDirective',function(){
    return {
        restrict:'E',// E : 标签指令, A: 属性指令
        template:'<div>hehe</div>',// 模板内容
        templateUrl:'test.html',// 外链模板
        replace:true,// 是否替换掉指令标签,而不是插入到标签中
        transclude:true,// 包含标签中原有的内容
        scope:true, // true : 代表独立作用域, 默认:false
        controller:function($scope){ // 此指令的控制器

        },
        controllerAs:'test'// 控制器别名
    }
});
```

## 自定义指令的作用域

默认自定义指令的作用域和父控制器的作用域是同一个， 当设置指令的`scope:true`的时候， 就会有了自己独立的作用域， 但是指令中的属性在没有和指令中的作用域中的属性绑定的时候还是会去父控制器的作用域中查找是否有该属性。当修改子控制器中的name的时候只会影响子控制器中的name, 不会影响父控制器中的name属性

```js
app.directive('test',function(){
    return {
        restrict:'EA',
        template:'<h1>{{name}}</h1><input type="text" ng-model="name" />',
        controller:function($scope){

        },
        scope:true // 独立作用域
    }
})
```

## 自定义指令传值

- 父控制器向指令中传值通过属性
- 指令中向父控制器中传值通过方法

```js
app.directive('test',function(){
    return {
        restrict:'EA',
        template:'<h1>{{name}}</h1><input type="text" ng-model="name" />',
        controller:function($scope){

        },
        scope:{
            // 父控制器向指令中单项传值, 指令中修改该属性不会影响父控制器中作用域
            // 等价于 name:'@name'
            // 如果name:'=name2' , 那么向指令中传值的时候需要使用name2这个属性
            name:'@', 

            // 双向传值,指令中修改该属性也会影响父控制器中作用域
            // 等价于 age:'=age'
            // 如果age:'=age2' , 那么向指令中传值的时候需要使用age2这个属性
            age:'=',
            // 指令向父控制器作用域中传值
            // 等价于 myclick:'&myclick'
            // 如果myclick:'$clickMe' ， 那么说明父控制器中的方法名是clickMe
            myclick:'&'
        },
        link:function($scope,ele,attrs){
            // ele : jqLite对象, 如果在angular.js之前引入了jquery, 那么这个ele就是jquery对象
            // attrs: 指令上的属性集合对象
        }  
    }
})
```


## 自定义过滤器

```js
var app = angular.module('app',[]);
app.filter('testfilter',function(){
    return function(v,params){
        // {{ 123 | testfilter : '-'}}
        console.log(v);//123
        console.log(params);// -
        return v;
    }
})
```

## 跨域

```js
var app = angular.module('app',[]);
app.controller('MainController',['$scope','$http',function($scope,$http){
    $http({
        url:'url地址'，
        method:'jsonp',
        params:{
            callback:'JSON_CALLBACK' // angular-1.5.8
        }
    }).success(function(data){
        console.log(data);
    }).error(function(err){
        console.log(err);
    })
}]);
```

## 服务Service
> 内置服务

- $location 对window.location的封装
```js
angular.module('test',[]).controller('testController',['$location',function($location){
    console.log($location.search()); // 路径中 #? 后的参数的json对象 {name:'liyajie',age:23}
    console.log($location.path());// 路径中第一个# 和第二个#之间的内容
    console.log($location.hash());// 路径中第二个 # 后面的内容
    console.log($location.url());//  第一个锚点后面的内容 # 后面的
}])
```

- $interval & $timerout

```js
angular.module('testTimeout',[]).controller('testController',['$scope','$timeout','$interval',function($scope,$timeout,$interval){
    $timeout(e => {
        $scope.name = 'lihaojie';
    },3000); // 3000ms之后执行里面的代码

    // 开启计时器
    var timer = $interval(e => {
        $scope.date = Date.now();
    },1000)

    $scope.stop = function(){
        $interval.cancel(timer);// 停止计时器
    }
}])
```

- $filter

```js
// 日期过滤器
var nowDate = Date.now();
var dateTime = $filter('date');
$scope.nowDate = dateTime(nowDate,'yyyy/MM/dd HH:mm:ss');

// 货币过滤器
$scope.money = $filter('currency')(123);
```

## 自定义服务

`value服务`

```js
var app = angular.module('app',[]);
app.controller('MainController',['$scope','key',function($scope,key){
    console.log(key); // liyajie
}]);
app.value('key','liyajie'); // 这样就设置了一个value服务， 类似于配置文件，一直不变的东西放到这里面
```

> factory方式自定义服务

```js
var app = angular.module('app',[]);

app.controller('MainController',['$scope','formatDate',function($scope,formatDate){
    $scope.currentDate = formatDate.formatDate();
}])

app.factory('formatDate',['$filter',function($filter){
    function formatDate(){
        var date = new Date();
        var dfilter = $filter('date');
        return dfilter(date,'yyyy-MM-dd HH:mm:ss');
    }

    return { formatDate: formatDate };
}])
```

> service方式自定义服务

```js
var app = angular.module('app',[]);

app.controller('MainController',['$scope','formatDate',function($scope,formatDate){
    $scope.currentDate = formatDate.formatDate();
}])

app.service('formatDate',['$filter',function($filter){
    this.formatDate = function(){
        var date = new Date();
        var dfilter = $filter('date');
        return dfilter(date,'yyyy-MM-dd HH:mm:ss');
    }
}])
```

## 表单验证

- 屏蔽浏览器的默认验证行为需要在form标签上添加`novalidate`属性

- `todoForm.$valid` : form名称.$valid  true: 说明表单验证通过, false: 说明表单验证没有通过

## $watch监听属性变化

> 语法: `$scope.$watch(监听的属性,属性改变后的回调函数, 是否是引用监控(默认false))`

```js
angular.module('app',[]).controller('MainController',['$scope',function($scope){
    $scope.arr = [];
    $scope.text = '';

    // 监听普通的属性
    $scope.$watch('text',function(newV,oldV){
        console.log(newV,oldV);
    },false);

    // 监听数组, 必须使用引用监控, 也就是第三个参数必须是true
    $scope.$watch('arr',function(newV,oldV){
        console.log(newV,oldV);
    },true);

    // Angular 1.1.4 出了`$watchCollection` 用来监控集合
    $scope.$watchCollection('arr',function(newV,oldV){
        console.log(newV,oldV);
    })
}]);
```

## 取消watch监听

```js
var unWatch = $scope.$watch('属性',function(newV,oldV){
    // 如果要取消监听执行unWatch就可以
});
// 取消监听
unWatch();
```

## 路由 angular-route

加载过来的内容存放位置：

```html
<ng-view></ng-view>
<!--或者-->
<div ng-view></div>
```

```html
<!--angular 1.5.8-->
<script src="/js/angular.js"></script>
<script src="/js/angular-route.js"></script>
```

```js
var app = angular.module('app',['ngRoute']);
app.controller('MainController','$routeParams',['$scope',function($scope,$routeParams){
    console.log($routeParams); // 这里的这个服务就是路由后面的参数 { id : 2}
}]);
app.config(['$routeProvider',function($routeProvider){
    $routeProvide.when('/home',{
        template:'<h1>I am Home</h1>'
    }).when('/about',{
        template:'<h1>I am About</h1>'
    }).when('/test/:id',{
        template:'<h1>我有参数</h1>',
        controller:'MainController'
    }).otherwise({
        redirectTo:'/home'
    });
}])
```


## AngularJs 1.5 和 1.6之间的差异

### $http服务请求

- 1.6之后的版本中的success和error方法已经去掉了
- 跨域的时候不在需要`params:{ callback:'JSON_CALLBACK' }`
- 跨域的时候需要配置白名单或者黑名单， 如下：
```js
app.config(['$sceDelegateProvider',function($sceDelegateProvider){
    $sceDelegateProvider.resourceUrlWhitelist([
        'self',
        'http://www.douban.com/**'
    ])
}]);
```

```js
$http({
    url:'',
    method:'get'
}).then(function(res){
    // res是一个对象， 后台返回的数据存放到了data属性中
    $scope.d = res.data;
}).cache(function(error){
    console.log(error);
});
```

### angular-route

angular-route中要使用导航的时候， 需要这样使用`<a href='#!/home'></a>`， href中需要使用`!/`

## 广播

> 父控制器中向指令中广播， 使用`$broadcast`

```js
angular.module('app').controller('mainCtrl',['$scope',function($scope){
    // 给指令发送一个广播
    $scope.$broadcast('广播名称',{name:'liyajie'})

    // 接收指令发来的广播
    $scope.$on('广播名称',function(e,obj){
        alert(obj.age);
    })
}])

// 指令中接收广播
angular.module('app').directive('test',function(){
    return {
        restrict:'EA',
        template:'<h1>test</h1>',
        controller:function($scope){
            // 接收父控制器发来的广播
            $scope.$on('广播名称',function(e,obj){
                alert(obj.name)
            })

            // 发送广播到父控制器
            $scope.$on('广播名称',{age:12});
        }
    }
})
```

## angular-ui-router

### 基本使用以及配置

```html
<ul>
    <li><a ui-sref-active='active' ui-sref='one'>one</a></li>
    <li><a ui-sref-active='active' ui-sref='two'>two</a></li>
    <li><a ui-sref-active='active' ui-sref='three'>three</a></li>
</ul>
```

```js
angular.module('app',['ui.router']).controller('mainCtrl',['$scope',function($scope){

}])
// 配置angular-ui-router
angular.module('app').config(['$stateProvider','$urlRouterProvider',function($stateProvider,$urlRouterProvider){
    $stateProvider.state('one',{
        url:'/one/:id',
        template:'<h1>one</h1>',
        controller:['$scope','$stateParams',function($scope,$stateParams){
            console.log($stateParams);// 获取参数
        }]
    }).state('two',{
        url:'/two',
        template:'<h1>two</h1>'
    }).state('three',{
        url:'/three',
        template:'<h1>three</h1>'
    });
    $urlRouterProvider.otherwise('two'); // 没有匹配到路由的情况下条状到 url:one的地址上去
}])
```

### 多视图

```html
<div ui-view='header'></div>
<div ui-view='left'></div>
<div ui-view='right'></div>
```

```js
angular.module('app',['ui.router']).controller('mainCtrl',['$scope',function($scope){

}])

angular.module('app').config(['$stateProvider','$urlRouterProvider',function($stateProvider,$urlRouterProvider){

    $stateProvider.state('one',{
        url:'/one',
        views:{
            header:{
                template:'<h1>header</h1>'
            },
            left:{
                template:'<h1>left</h1>'
            },
            right:{
                template:'<h1>right</h1>'
            }
        }
    })

    // 配置匹配不到路由的时候
    $urlRouterProvider.otherwise('one');
}])
```

### 多视图嵌套

```html
<div ui-view='header'></div>
<div ui-view='left'></div>
<div ui-view='right'></div>
```

```js
angular.module('app',['ui.router']).controller('mainCtrl',['$scope','$state',function($scope,$state){
    // 跳转到指定的state , 也可以添加到主路由的控制器上。
    $state.go('two.login');
}])

angular.module('app').config(['$stateProvider','$urlRouterProvider',function($stateProvider,$urlRouterProvider){

    $stateProvider.state('one',{
        url:'one',
        views:{
            header:{
                template:'<h1>header</h1>'
            },
            left:{
                template:'<h1>left</h1>'
            },
            right:{ // 视图中嵌套视图
                template:`
                <ul>
                    <li><a ui-sref='two.login'>login</a></li>
                    <li><a ui-sref='two.reg'>reg</a></li>
                </ul>
                <div ui-view></div>
                `
            }
        }
    }).state('two',{
        url:'/two',
        template:'<h1>two</h1>',
        controller:['$scope','$state',function($scope,$state){
            // 跳转到指定路由。
            $state.go('two.reg');
        }]
    }).state('two.login',{
        url:'login',
        template:'<h1>login</h1>'
    }).state('two.reg',{
        url:'reg',
        template:'<h1>reg</h1>'
    })

    // 配置匹配不到路由的时候
    $urlRouterProvider.otherwise('one');
}]);
```

## $q.defer

```js
angular.module('app',[]).controller(['$scope','$http','$q',function($scope,$http,$q){
    var def = $q.defer()
    $http({
        url:'',
        method:'get'
    }).then(function(data){
        def.resolve(data)
    }).catch(function(err){
        def.reject(err)
    })
    def.promise.then(function(data){
        console.log(data)
    },function(err){
        console.log(err)
    },function(update){
        console.log(update)
    })
}])
```
