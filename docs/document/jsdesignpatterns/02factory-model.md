## 介绍
工厂模式, 顾名思义就是使用一个工厂来生产产品, 而产品在程序中就是我们说的对象,而在创建对象的时候无须指定具体的类.
工厂模式定义了一个用于创建对象的接口, 这个接口由子类决定实例化哪一个类, 该模式使一个类的实例化延迟到了子类, 而子类也可以重写接口以便在创建的时候指定自己的对象类型. 

## 工厂模式简单示例
```js
// 产品管理者
var productManager = {};
// 用于创建产品A
productManager.createPA = function(){
	console.log('A');
}
// 用于创建产品B
productManager.createPB = function(){
	console.log('B');
}
// 产品工厂
productManager.fac = function(type){
	return new productManager[type];
}
// 创建产品B
productManager.fac('createPB');
// 结果打印出B
```

## 工厂模式实现需求
	需求: 我们要给页面插入文本 , 图片, 链接
```js
var page = page || {};
page.dom = page.dom || {};
// 插入文本的方法
page.dom.Text = function(){
	this.insert = function(where){
		var txt = document.createTextNode(this.url);
		where.appendChild(txt);
	}
}
// 插入图片的方法
page.dom.Image = function(){
	this.insert = function(where){
		var img = document.createElement('img');
		img.src = this.url;
		where.appendChild(img);
	}
}
// 添加链接的方法
page.dom.Link = function(){
	this.insert = function(where){
		var link = document.createElement('a');
		link.href = this.url;
		link.appendChild(document.createTextNode(this.url));
		where.appendChild(link);
	}
}
// 创建工厂方法
page.dom.factory = function(type){
	return new page.dom[type];
}
var o = page.dom.factory('Link');
o.url = 'http://www.liyajie.net/';
o.insert(document.body);
var t = page.dom.factory('Text');
t.url = '李亚杰';
t.insert(document.body);
var i = page.dom.factory('Image');
i.url = 'http://www.liyajie.net/favicon.ico';
i.insert(document.body);
```