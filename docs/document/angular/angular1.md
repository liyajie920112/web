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