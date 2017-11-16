---
title: es6之箭头函数
date: 2017-06-17 09:10:24
tags: es6
---
### 基本语法
ES6允许使用“箭头”（=>）定义函数。
``` bash
var f = v => v;
//等同于
var f = function(v) {
  return v;
};
```
如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。
``` bash
var f = () => 5;
// 等同于
var f = function () { return 5 };

var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};
```
如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。
``` bash
var sum = (num1, num2) => { return num1 + num2; }
```
由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号。
``` bash
var getTempItem = id => ({ id: id, name: "Temp" });
```
箭头函数可以与变量解构结合使用。
``` bash
const full = ({ first, last }) => first + ' ' + last;
// 等同于
function full(person) {
  return person.first + ' ' + person.last;
}
```
### 使用注意点
箭头函数有几个使用注意点。

（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替。

（4）不可以使用yield命令，因此箭头函数不能用作Generator函数。

this指向的固定化，并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，导致内部的this就是外层代码块的this。正是因为它没有this，所以也就不能用作构造函数。
除了this，以下三个变量在箭头函数之中也是不存在的，指向外层函数的对应变量：arguments、super、new.target。
``` bash
function foo() {
  setTimeout(() => {
    console.log('args:', arguments);
  }, 100);
}

foo(2, 4, 6, 8)
// args: [2, 4, 6, 8]
```
上面代码中，箭头函数内部的变量arguments，其实是函数foo的arguments变量,另外，由于箭头函数没有自己的this，所以当然也就不能用call()、apply()、bind()这些方法去改变this的指向。
``` bash
(function() {
  return [
    (() => this.x).bind({ x: 'inner' })()
  ];
}).call({ x: 'outer' });
// ['outer']
```
上面代码中，箭头函数没有自己的this，所以bind方法无效，内部的this指向外部的this。长期以来，JavaScript语言的this对象一直是一个令人头痛的问题，在对象方法中使用this，必须非常小心。箭头函数”绑定”this，很大程度上解决了这个困扰。

### 箭头函数与常规函数对比
一个箭头函数与一个普通的函数在两个方面不一样：

下列变量的构造是词法的： arguments ， super ， this ， new.target
不能被用作构造函数：没有内部方法 [[Construct]] （该方法允许普通的函数通过 new 调用），也没有 prototype 属性。因此， new (() => {}) 会抛出错误。
除了那些意外，箭头函数和普通的函数没有明显的区别。例如， typeof 和 instanceof 产生同样的结果：
``` bash
typeof () => {}
//'function'
() => {} instanceof Function
//true

typeof function () {}
//'function'
function () {} instanceof Function
//true
```
函数表达式和对象字面量是例外，这种情形下必须放在括号里面，因为它们看起来像是函数声明和代码块。