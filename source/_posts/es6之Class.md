---
title: es6之Class
date: 2017-07-24 22:10:24
tags: es6
---

### Class 
- 1.在之前的JS面向对象编程中，如果定义一个构造函数，一般来说是这样：
``` bash
function Person( name , age ) {
    this.name = name;
    this.age = age;
}
Person.prototype.say = function(){
    return 'My name is ' + this.name + ', I am ' + this.age + ' years old';
}
```
这种写法跟传统的面向对象语言（比如 C++ 和 Java）差异很大，对于一些新学习JS的程序员来说不太容易理解，ES6 引入了 Class（类）这个概念，提供了更接近传统语言的写法，上述代码用 ES6 实现就是：
``` bash
class Person {
  constructor( name , age ) {
    this.name = name;
    this.age = age;
  }

  say() {
    return return 'My name is ' + this.name + ', I am ' + this.age + ' years old';
  }
}
```
上述代码中的constructor方法，这就是构造方法，而this关键字则代表实例对象

- 2.constructor方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类必须有constructor方法，如果没有显式定义，会被默认添加一个空的constructor方法；
``` bash
class Person {
}

// 等同于
class Person {
  constructor() {}
}
```
- 3.constructor方法默认返回实例对象（即this），完全可以指定返回另外一个对象，但实际开发中不建议这么做；
``` bash
class Foo {
  constructor() {
    return Object.create(null);
  }
}

new Foo() instanceof Foo  //false
```
此时constructor函数返回一个全新的对象，结果导致实例对象不是Foo类的实例，因此会返回 false；

- 4.使用new运算符用于生成类的实例对象，如果忘记加上new，像函数那样调用Class，将会报错；

``` bash
class Person {
  // ...
}

// 报错
var person = Person();

// 正确
var person = new Person();
```

- 5.实例的属性除非显式定义在其本身（即定义在this对象上），否则都是定义在原型上（即定义在class上），并且，类的所有实例共享一个原型对象
``` bash
class Person {

  //自身属性
  constructor( name , age ) {
    this.name = name;
    this.age = age;
  }

  //原型对象的属性
  say() {
    return 'My name is ' + this.name + ', I am ' + this.age + ' years old';
  }
}

var person = new Person( 'Jack' , 23);
person.hasOwnProperty('name')                   // true
person.hasOwnProperty('age')                    // true
person.hasOwnProperty('say')                    // false
person.__proto__.hasOwnProperty('say')          // true
```
上述代码中，name 和 age 实例对象person自身的属性（因为定义在this变量上） ，所以hasOwnProperty方法返回true，而say是原型对象的属性（因为定义在Person类上），所以hasOwnProperty方法返回false；

- 6.与函数一样，可以使用表达式的形式定义一个类，即
``` bash
const MyClass = class Me {
  getClassName() {
    return Me.name;
  }
}
```
值得注意的是，这个类的名字是MyClass而不是Me，Me只在 Class 的内部代码可用，用来指代当前类；如果类的内部没用到的话，可以省略Me，写成下面的形式：
``` bash
const MyClass = class {
  getClassName() {
    return Me.name;
  }
}
```

- 7.采用 Class 表达式，可以写出立即执行的 Class。
``` bash
let Person = new class {
  constructor(name , age ) {
    this.name = name;
    this.age = age;
  }

  say() {
    return 'My name is ' + this.name + ', I am ' + this.age + ' years old';
  }
}('Jack' , 23 );

Person.say();   // 'My name is Jack, I am 23 years old'
```

- 8.ES6中类不存在变量提升，不会把类的声明提升到代码头部，即如果类使用在前，定义在后，这样会报错；
``` bash
new Person();   //Uncaught ReferenceError : Person is not defined
class Person{}
```

- 9.跟ES5一样，ES6的类定义中，也不支持私有属性和私有方法，只能通过变通的方法模拟实现（命名上加下划线等）；
- 10.可以在一个类的方法前，加上static关键字，声明其为‘静态方法’，这样就表示该方法不会被实例继承，而是直接通过类来调用；
``` bash
class Foo {
  static classMethod() {
    return 'Hello World';
  }
}

Foo.classMethod()   // 'Hello World'

var foo = new Foo();
foo.classMethod()   // TypeError: foo.classMethod is not a function
```

- 11.父类的静态方法，可以被子类继承，也可以从super对象上调用。
``` bash
class Parent {
  static classMethod() {
    return 'Hello World';
  }
}

//子类继承
class Child extends Parent {
}

Child.classMethod() // 'Hello World'

//super对象上调用
class Child extends Parent {
  static classMethod() {
    return super.classMethod();
  }
}

Child.classMethod()  // 'Hello World'
```

- 12.静态属性指的是 Class 本身的属性，即Class.propName，而不是定义在实例对象（this）上的属性，并且目前只能有以下方式定义：
``` bash
class Foo {
}

Foo.prop = 1;
Foo.prop // 1
```

- 13.Class 可以通过extends关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多；
``` bash
class Parent {
  //
}

//子类继承
class Child extends Parent {
}
```
上面代码定义了一个Child类，该类通过extends关键字，继承了Parent类的所有属性和方法；

- 14.子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象；
``` bash
class Parent( name , age ) {
  //
}

//子类继承
class Child extends Parent {
    constructor( name , age , hobby ) {
        super( name , age );  // 调用父类的constructor( name , age )
        this.hobby = hobby;
    }

    say(){
        return 'My name is ' + this.name + ', I am ' + this.age + ' years old , I like ' + this.hobby;
    }
}
```

- 15.super关键字，既可以当作函数使用，也可以当作对象使用：

	- 作为函数调用时，代表父类的构造函数，并且只能用在子类的构造函数之中，用在其他地方就会报错；
``` bash
  class A {}
  class B extends A {
    m() {
          super();        // 报错
    }
  }
```
	- 作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类
``` bash
class A {
    p() {
      return 2;
    }
  }
  class B extends A {
    constructor() {
      super();
      console.log(super.p()); // 2
    }
  }
  let b = new B();

```
需要注意的是，super指向的是父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过super调用的
``` bash
class A {
  constructor() {
    this.p = 2;
  }
}

class B extends A {
  get m() {
    return super.p;
  }
}

let b = new B();
b.m                 // undefined
```

- 16.ES6 规定，通过super调用父类的方法时，super会绑定子类的 this 
``` bash
class A {
  constructor() {
    this.x = 1;
  }
  print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  m() {
    super.print();
  }
}

let b = new B();
b.m() // 2
```

上面代码中，super.print()虽然调用的是A.prototype.print()，但是A.prototype.print()会绑定子类B的this，导致输出的是2，而不是1。也就是说，实际上执行的是super.print.call(this)。

- 17.由于绑定子类的this，因此如果通过super对某个属性赋值，这时super就是this，赋值的属性会变成子类实例的属性；
``` bash
class A {
  constructor() {
    this.x = 1;
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
    super.x = 3;
  }

  print(){
     console.log(super.x);  // undefined
     console.log(this.x);   // 3
  }
}

let b = new B();
b.print();
```
上面代码中，super.x 赋值为3，这时等同于对this.x赋值为3。而当读取super.x的时候，读的是A.prototype.x，而A的原型上没有x属性，所以返回undefined。

- 18.如果super作为对象，用在静态方法之中，这时super将指向父类，而不是父类的原型对象。
``` bash
class Parent {
  static myMethod(msg) {
    console.log('static');
  }

  myMethod(msg) {
    console.log('instance');
  }
}

class Child extends Parent {
  static myMethod() {
    super.myMethod();
  }

  myMethod(msg) {
    super.myMethod();
  }
}

Child.myMethod(); // static 

var child = new Child();
child.myMethod(); // instance
```
上述代码表明super在静态方法之中指向父类，在普通方法之中指向父类的原型对象。

- 19.Class 作为构造函数的语法糖，同时有prototype属性和 _proto_属性，因此同时存在两条继承链
	- 子类的_proto_属性，表示构造函数的继承，总是指向父类。
	- 子类prototype属性的_proto_属性，表示方法的继承，总是指向父类的prototype属性
``` bash
 class A {
 }
 class B extends A {
 }
 B._proto_ === A // true
 B.prototype._proto_ === A.prototype // true
```

- 20.extends关键字不仅可以用来继承类，还可以用来继承原生的构造函数。因此可以在原生数据结构的基础上，定义自己的数据结构，下面就是定义了一个带版本功能的数组例子。
``` bash
class VersionedArray extends Array {
  constructor() {
    super();
    this.history = [[]];
  }
  commit() {
    this.history.push(this.slice());
  }
  revert() {
    this.splice(0, this.length, ...this.history[this.history.length - 1]);
  }
}

var x = new VersionedArray();

x.push(1);
x.push(2);
x // [1, 2]
x.history // [[]]

x.commit();
x.history // [[], [1, 2]]

x.push(3);
x // [1, 2, 3]
x.history // [[], [1, 2]]

x.revert();
x // [1, 2]
```