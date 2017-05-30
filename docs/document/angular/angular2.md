## AngularCli

```bash
npm install -g @angular/cli

ng new project-name

cd project-name

npm install

ng serve
```

### ng serve
- 启动这个服务
- `ng serve --prod --aot` 用于预编译和生产, 这样会压缩代码

### ng generate
- 通过命令帮我们创建模板

ng generate 简化版 ng g

```bash
cl:class
c:component
d:directive
e:enum
g:guard
i:interface
m:module
p:pipe
s:service
```

```bash
# 创建一个user组件
ng g c user
```

### ng build
- 将我们的程序打包到`dist`目录中
- `ng build --prod --aot`  压缩并打包

## NgModule
- 一般的组成
    - Component
    - Directive
    - Service
    - Route

## DI 依赖注入
- Angular2中只有一种注入方式: 构造器注入.
- @injectable是@Component的子类
- 每一个html标签上都会有一个注射器实例

注射器(injector)也是一个树形结构

```html
<tab> <!--Parent injector -->
    <panel title='one'></panel><!--child injector-->
    <panel title='two'></panel>
</tab>
```

## UILibraries

- ng2-bootstrap
- PrimeNG
- Ionic
- Angular-Material2

## 事件绑定

```html
<!--双向绑定,使用 [()]-->
<input type='text' [(ngModel)]='text' />

<!--事件绑定使用 ()-->
<button (click)='clickFun()'>click</button>
```

## 遍历指令 *ngFor

```html
<ul>
    <li *ngFor='let item of items'>...</li>
</ul>
```

## ngClass

```html
<!--绑定单个class, 当selected === id 的时候, selected这个class才会添加上-->
<div [class.selected]='selected === id'>...</div>

```

绑定多个class

```js
currentClasses: {};
setCurrentClasses() {
  // CSS classes: added/removed per current state of component properties
  this.currentClasses =  {
    saveable: this.canSave,
    modified: !this.isUnchanged,
    special:  this.isSpecial
  };
}
```
```html
<div [ngClass]="currentClasses">This div is initially saveable, unchanged, and special</div>
```

## 声明一个组件

- 创建一个组件.ts文件, 名字采用中划线的方式, 最后要以.component.ts结尾, 如:`hero-detail.component.ts`, 对应的组件名称则是:`HeroDetailComponent`

- 导入`@angular/core`, 需要里面的`Component` 和 `Input`
```js
import { Component , Input } from '@angular/core'
@Component({
    selector:'hero-detail', // 说明白点就是自定义的标签名称
    template:`<h1>...</h1>`, // 模板内容
    styles:[...] // 该组件的样式, 这种方式该样式只能在该组件中使用, 数组中可以是样式文件的相对路径
})

export class HeroDetailComponent {
    @Input() hero: Hero; // 定义一个Hero类型的变量hero, @Input 说明可输入的类型, 因为父组件向该子组件传递数据的时候该属性必须是可输入的
}
```

## 父组件向子组件传递消息

```ts
@Component({
    selector:'<app-root></app-root>',
    // 通过 [hero] = 'selectedHero' 这种方式传递数据, hero是子组件中的属性, 必须是可输入的.
    template:`<div><hero-detail [hreo]='selectedHero'></hero-detail></div>`
})
```

## 服务 Injectable
- 服务就是给我们提供服务的 , 需要什么服务它就提供什么服务.
- 服务名称一般是以 `.service.ts`结尾


> 创建一个服务

```ts
import { HEROES } from './heroes'
import { Injectable } from '@angular/core';

@Injectable()
export class HeroesService {
    getHeroes() : Promise<Hero[]> {
        return Promise.resolve(HEROES);
    }
}
```

> 使用我们上面创建的服务

```ts
import { HeroService } from './hero.service'
import { Component, OnInit } from '@angular/core'
import { Hero } from './hero'

@Component({
    selector:'app-root',
    providers:[ HeroService ],// 添加 providers
    template:'<h1>...</h1>'
})

// 注入这个服务
export class AppComponent implements OnInit {
    // 使用构造器注入
    constructor(private heroService : HeroService){};
    heroes:Hero[];
    getHeroes():void{
        // 因为服务中用到的是Promise的方式, 所以这里要用 then的方式, 这是一种异步加载技术
        this.heroService.getHeroes().then(data => this.heroes = data);
    }
    ngOnInit():void { // 声明周期钩子函数, 初始化的时候获取英雄列表
        this.getHeroes();
    }
}
```

> 如果我们把服务添加到AppModule的providers数组中, 那么这个服务就是单例的, 所有的组件都可以使用它.

## 路由RouterModule(@angular/router)

> 路由的配置

- 在index.html中添加bese

```html
<base href='/' />
```

```ts
// 导入RouterModule
import { RouterModule } from '@angular/router';

RouterModule.forRoot([
    {
        path:'heroes',
        component:ComponentName
    }
])
```
- **Path** : 路由器会用它匹配浏览器地址栏中的地址, 如:heroes
- **Component** : 当匹配到的时候, 路由器需要创建组件: ComponentName

> 要想路由可用需要添加到NgModule的imports中

```ts
@NgModule({
    // 模块声明
    declarations:[
        AppComponent
    ],
    // 依赖的模块
    imports:[
        RouterModule.forRoot([
            {
                path:'heroes',
                component:ComponentName
            }
        ])
    ],
    // 依赖的服务
    providers:[ HeroService ],
    // 手动启动应用
    bootstrap:[ AppComponent ]
})
```

> 这里使用了`forRoot()`方法，因为我们是在应用根部提供配置好的路由器。 `forRoot()`方法提供了路由需要的路由服务提供商和指令，并基于当前浏览器 URL 初始化导航。

!> 配置好路由之后, 要使用路由必须要有`<router-outlet></router-outlet>`标签, 用来放路由匹配到的组件内容, 并且`a`链接需要这样导航`<a routerLink='/heroes'></a>`

## 路由配置重定向

需要使用`redirectTo`属性

```ts
imports : [
    {
        path:'',
        redirectTo:'/index',// 重定向的页面, 注意, 这个路径也需要配置好, 否则重定向过去的时候会找不到
        pathMatch:'full'
    },{
        path:'index',
        component:ComponentName
    }
]
```

### navigate

```ts
import { Component,OnInit } from '@angular/core';
import { Router } from '@angular/router';
@Component({
    ...
})
export class TestComponent implements OnInit {
    constructor(private router:Router){};
    gotoDetail(): void{
        // 跳转到/detail/:id页面
        this.router.navigate(['/detail',id]);
    }
}
```

### routerLinkActive指令
- 如果是当前路由, 则会添加一个激活状态的class, className就是active, 如下:

```html
<a routerLink='/detail' routerLinkActive='active'>当前页面</a>
```

## Location
- 回到上一个页面

```ts
import { Location } from '@angular/common';

@Component({
    ...
})

export class AppComponent{
    constructor(private location:Location){};

    goBack(): void{
        this.location.back();// 回到上一个页面
    }
}
```
