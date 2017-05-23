## Angular指令
- ng-init
- ng-app
- ng-controller
- ng-hide
- ng-if
- ng-show
- ng-repeat
- ng-class
- ng-src
- ng-include
    - 使用Ajax获取到指定的模板内容, 插入到标签中
- ng-transclude
    - 自定义标签指令中, 我们可以使用ng-transclude指定指令模板中的哪部分包含,哪部分不包含, 需要在自定义指令中使用`transclude:true|false`属性进行控制


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