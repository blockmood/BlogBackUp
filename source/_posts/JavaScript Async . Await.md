---
title: JavaScript Async/Await
date: 2017-08-01 16:10:24
tags: javascript
---
### 什么是Async/Await？

Async/Await是一个很久就令人期待的JavaScript功能，它让使用异步函数更加愉快和容易理解。它是基于Promises的并且和现存的所有基于Promises的API相兼容。

从async和await这两个名字来的这两个关键字将会帮助我们整理我们的异步代码。

Async - 声明一个异步函数（async function someName(){…}）。

- 自动将一个常规函数转化为一个Promise。
- 当调用async函数的时候，它用函数内返回的任何值来解决（resolve）。
- 异步函数可以使用await

Await-暂停执行async函数（var result = await someAsyncCall();）。

- 当放在一个Promise前面执行，await强制剩下的代码等待直到那个Promise结束并且返回一个结果
- Await只有和Promise一起使用才有用，和回调函数（普通函数）一起使用不会产生作用
- Await只可以在async函数内部使用

这里有一个简单的例子，希望能帮你把事情理清楚：

假设我们想从我们的服务器中得到一些JSON文件。我们将写一个函数用axios库发送一个http GET 请求到https://tutorialzine.com/misc/files/example.json。 我们不得不等待服务器响应，所以很自然这个HTTP 请求是异步的。

下面我们可以看到相同的功能实现了两次。首先是用Promises，然后第二次用Async / Await。

``` bash
// Promise 实现方式
function getJSON(){

    // 为了让函数阻塞我们手动创建了一个Promise
    return new Promise( function(resolve) {
        axios.get('https://tutorialzine.com/misc/files/example.json')
            .then( function(json) {
                // 从请求来的数据可以从.then中得到
                // 我们使用resolve返回结果
                resolve(json);
            });
    });

}

// Async/Await 实现方式
// async关键字将会自动创建一个新的Promise并返回。
async function getJSONAsync(){
    // await关键词使我们免于写一个.then()
    let json = await axios.get('https://tutorialzine.com/misc/files/example.json');
    // GET请求的结果可以从json变量中得到
    // 我们把结果就像一个普通的同步函数一样返回
    return json;
}
```

很明显Async/Await版本的代码更短并且可读性更强。除了使用的语法，两个函数完全相同-他们都返回Promises并且都从axios得到JSON返回。我们可以像这样调用我们的async函数：

``` bash
getJSONAsync().then( function(result) {
    // Do something with result.
});
```

### 那么，Async/Await 让 Promises 过时了吗？

一点也不，当我们使用Async/Await其实底层还是在使用Promises。对Promises的良好理解在长远考虑下是对你是十分有帮助的并且也是高度推荐的。

这里甚至有一些情况Async/Await不能解决而我们不得不重新去寻求Promises的帮助。一种这样的场景是我们需要去调用多个独立的异步函数并等待他们所有完成。

假如我们尝试用async and await，以下就会发生:

``` bash
async function getABC() {
    let A = await getValueA(); // getValueA takes 2 second to finish
    let B = await getValueB(); // getValueB takes 4 second to finish
    let C = await getValueC(); // getValueC takes 3 second to finish
    return A*B*C;
}
```

每一个await将会等待前一个返回一个结果。因为我们一次只执行一个调用那么整个函数从开始到结束将会花9秒的时间（2+4+3）。

这不是一个最佳的解决方案，因为A,B和C互相并不依赖。换句话来说我们在得到B的时候我们并不需要A的值。我们可以同时得到这些值以减去几秒钟的等待时间。

同时发送所有的requests我们需要Promise.all()。这将会保证我们在进行下一步的时候我们可以得到所有的结果，但是所有的异步函数将会平行的运行，而不是一个接一个。

``` bash
async function getABC() {
  // Promise.all() allows us to send all requests at the same time. 
  let results = await Promise.all([ getValueA, getValueB, getValueC ]); 

  return results.reduce((total,value) => total * value);
}
```

这种方式函数将会花费少的多的时间，在getValueB结束的时候getValueA和getValueC就已经结束了。我们将有效的减少执行的时间到最慢的请求而不是时间的总和。

### 处理Async/Await的错误

另一个关于Async/Await很棒的事是它允许我们用很棒的老式try/catch块去catch任何意外的错误。

``` bash
async function doSomethingAsync(){
    try {
        // This async call may fail.
        let result = await someAsyncCall();
    }
    catch(error) {
        // If it does we will catch the error here.
    }  
}
```

任何我们写在try块中的等待的异步调用或者其他任何错误代码，catch都能解决他们引起的错误（error）。

如果情况需要，我们也可以在执行async函数的时候抓取错误。由于所有异步函数返回Promises，所以我们可以在调用它们时简单地包含.catch()事件处理程序。

``` bash
// 不包含try/catch块的async函数
async function doSomethingAsync(){
    // 这个async调用也许会失败
    let result = await someAsyncCall();
    return result;  
}

// 我们catch错误在调用async函数的时候
doSomethingAsync().
    .then(successHandler)
    .catch(errorHandler);
```

重要的是选择一种你喜欢的错误处理的方式，并坚持使用它。同时使用try/catch和.catch(）将很有可能导致一些问题。

### 浏览器支持

Async/Await已经支持大部分的主流浏览器。绝大多数的厂商将会识别你的async/await代码而不需要额外的库-除了IE11.

Node开发者只要node8或以上就可以享受到改进的异步流。它在今年晚些时候应该会变成LTS（ Long Term Support ）版本。

如果这兼容性不能满足你，也有许多像Babel和TypeScript和Nodejs库asyncawait这样的JS transpiler（源码转换器）提供他们自己的跨平台版本的功能。