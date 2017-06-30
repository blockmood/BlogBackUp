---
title: javascript中的Function和Object
date: 2017-06-29 10:10:24
tags: javascript
---
在JavaScript中所有的对象都继承自Object原型，而Function又充当了对象的构造器，那么Funtion和Object到底有着什麽样的关系呢 ？
首先，一切都是对象。
``` bash
alert(Object instanceof Object); // true  
alert(Function instanceof Object);// true    
alert(Object instanceof Function);// true   
alert(Function instanceof Function);// true   
```
由此可见，Object继承自己，Funtion继承自己，Object和Function互相是继承对方，也就是说Object和Function都既是函数也是对象。这一点很特别。所有的函数都是对象，可是并不是所有的对象都是函数。证明如下：
``` bash
function foo(){};  
alert(foo instanceof Function); // true  
alert(foo instanceof Object); // true  
alert(new foo() instanceof Function); // false  
```

我们看到由new + function的构造器实例化出来的对象不是函数，仅仅是Object的子类。接下来，我们会想所有的对象都有一个Function的构造器存储在原型链的constructor属性中，那么Object的构造器是什麽呢？ 证明：
``` bash
alert(Object.constructor); // function Function(){ [native code] }  
alert(Function.constructor); // function Function(){ [native code] }  
```
由此我们可以确定Object是由Function这个函数为原型的，而Function是由它自己为原型的。Function函数是由native code构成的，我们不用去深究了。存在function Function(){...}的定义，我们明白了Function其实就是函数指针，也看作函数变量。就相当于function foo(){}中的foo。连Object的构造器都是指向Function的，可以想象Function是一个顶级的函数定义，大大的区别于自定义的如function foo(){}这样的函数定义。
 
看这样一个语句，new Function();以Function为原型来实例化一个对象，按照刚才Object.constructor 为 functon Function(){}来说，new Function()产生的对象应该是一个Object啊，我们来验证一下：
``` bash
alert(new Function() instanceof Object); // true  
alert(Object instanceof new Function());// false  
alert(typeof new Function());// function  
alert(typeof Object);// function  
```
其实new Function();将产生一个匿名函数，由于Function是顶级函数所以可以产生自定义函数，我们可以把所有的函数看作Function的子类。但所有的一切包括函数都是Object的子类这点是不变的，也得到了体现。typeof Object的结果说明Object也是一个函数。继续做实验：
``` bash
alert(new Object().constructor); // function Object(){ [native code] }  
```
一个情理之中的疑惑。可以这么说凡是可以放在new后面的都是一个函数构造器，那么Object确实也像其它函数一样，是一个函数的指针或者是函数变量，但Function是顶级的所以Object要由Function来构造。可以这么理解，Function和Object一个是上帝，一个是撒旦同时诞生于宇宙的最开始，拥相当的力量。但是上帝更为光明，所以高高在上。Object要由Function来构造，Function属于顶级函数。但是撒旦并没有绝对的输给上帝，否则上帝就会消灭撒旦。于是或所有的对象都要继承Object包括Function(Object和Function既是对象又是函数)。就相当于所有的人包括上帝都有邪念一样。
 
拓展一下，由Object我们会想到Array,Number,String等这些内置对象。有理由相信这些都是Object的子类。如下：
``` bash
alert(Array instanceof Object) // true  
alert(String instanceof Object) // true  
alert(Number instanceof Object) // true  
alert(Object instanceof Array) // false  
alert(Object instanceof String) // false  
alert(Object instanceof Number) // false  
```
当然他们也都会有Object的特性就像魔鬼和撒旦的关系一样，也是Function的子类，由Function构造。那么有Array,String,Number构造的对像如：new Array();new Number();new String()的构造器是function Array(){...};function String(){...};function Number(){...};
``` bash
alert(Array instanceof Function) // true  
alert(String instanceof Function) // true  
alert(Number instanceof Function) // true  
alert(Array.constructor) // function Function(){ [native code] }  
alert(String.constructor) // function Function(){ [native code] }  
alert(Number.constructor) // function Function(){ [native code] }  
```
总结一下，像内置的函数或说对象把如:Object,String,Array等等和自定义的function关键字定义的函数,都是Function的子类。new Function()相当于function关键字定义。这里可以引出，Function.prototype原型链上的属性所有函数共享,Object.prototype原型链上的属性所有对象共享。