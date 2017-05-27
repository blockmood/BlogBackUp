---
title: JavaScript之原型详解
date: 2017-05-24 08:10:24
tags: javascript
---
本篇文章主要介绍的是函数对象中的原型(prototype)属性,对于JavaScript,理解原型是一个非常重要的环节，所以我今天把自己的经验分享出来，先看一段代码。

### 原型属性
``` bash
function foo(a,b){

}
console.log(foo.lenght);    		  //输出2
console.log(foo.constructor);   	  //输出Function
console.log(typeof foo.constructor);  //输出Object
```
以上的代码，其实在函数定义时就被创建了，属性中就包括prototype属性，它的初始值是一个空的对象。
当然我们也可以自己添加属性，如:
``` bash
foo.prototype = {}
```
我们可以赋予这个空对象一些属性和方法，并且与foo函数本身不会造成什么影响。
### 利用原型添加属性和方法
定义一个构造函数，并用构造函数来创建出构造对象，这种的做法是通过new关键字返回一个函数的对象，利用这个对象就可以调用this返回其对象的方法或者属性，这样就有了一种赋予对象一种新的方法或者属性的功能。
``` bash
function Gadget(name,color){
	this.name = name;
	this.color = color;
	this.say = function(){
		return this.name + this.color;
	}
}
Gadget.prototype = {
	price:100,
	rating:3,
	getInfo:function(){
		return this.price + this.rating;
	}
}
```
向原型中添加完属性和方法之后，就可以访问了，这里构造函数自身的属性优先级要高于原型的属性。
``` bash
var newtoy = new Gadget('yuexing','blue');
console.log(newtoy.say());   //输出 yuexing blue
console.log(newtoy.name);    //输出yuexing
console.log(newtoy.price);   //输出100
console.log(newtoy.getInfo());   //输出100 3
```
对于原型来说，最重要的一个概念就是“驻留”，由于在javascrit中，对象都是通过引用方式来传递值的，也就是传递的实际是一个地址，因此我们所创建的对象中的原型，没有一份属于自己的原型副本，这就意味着我们随时可以对原型进行添加和修改，并且对象会继承者原型这一切的改变。
``` bash
Gadget.prototype.get = function(what){
	return this[what];
}
var newtoy = new Gadget('yuexing','blue');
console.log(newtoy.get('price'));    //输出100
```
### 自身属性和原型属性
在之前的getInfo方法中，我你们用的是this指针来完成对象访问的，但是执行使用Gadget.prototype也可以直接访问,这难道有什么不同么？
当我们访问newtoy的某个属性的时候，JavaScript引擎会遍历该对象的所有属性，并查找一个叫name 和 color的属性，如果找到了就立即返回值，如果找不到，Javascrit引擎就会去查询创建当前对象的原型属性，等价于我们直接访问（Gadget.prototype），如果在原型中找到了就立即返回原型中的值，这个结构会一直持续下去，并且最终取决于原型链的长度，但其最后肯定是Object对象，因为他是最高级的父级对象。至于它为什么会向上查找，因为这里有一个神秘的_ _proto_ _的属性，下节再说这个属性。