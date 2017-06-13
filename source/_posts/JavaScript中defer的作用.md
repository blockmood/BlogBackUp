---
title: JavaScript中defer的作用
date: 2017-06-13 13:10:24
tags: javascript
---
很多人都已经把 Javascript的用的炉火纯青了，但见到defer未必就知道他是做什么用的；很多人也都遇到过这样的问题，需要直接执行别且操作DOM对象的js 总是报找不到对象的错误，原因大家也都知道就是页面还有没有加载完毕，js的操作对象还在下载中。但很多人都不知道，添加defer标签就能轻而易举的解决这个问题。
``` bash
<script src="../CGI-bin/delscript.js" defer></script>
中的defer作用是文档加载完毕了再执行脚本,这样回避免找不到对象的问题---有点问题

<button id="myButton" onclick="alert('ok')">test</button>
<script>
myButton.click();
</script>

<script>
myButton.click();
</script>
<button id="myButton" onclick="alert('ok')">test</button>

<script defer>
function document.body.onload() {
alert(document.body.offsetHeight);
}
</script>
```
加上 defer 等于在页面完全在入后再执行，相当于 window.onload ，但应用上比 window.onload 更灵活！
defer是脚本程序强大功能中的一个“无名英雄”。它告诉浏览器Script段包含了无需立即执行的代码，并且，与SRC属性联合使用，它还可以使这些脚本在后台被下载，前台的内容则正常显示给用户。
--但是 文档加载完毕了再执行脚本

请注意两点：
1、不要在defer型的脚本程序段中调用document.write命令，因为document.write将产生直接输出效果。
2、而且，不要在defer型脚本程序段中包括任何立即执行脚本要使用的全局变量或者函数。 

一个常用的优化性能的方法是：当脚本不需要立即运行时，在`<SCRIPT>`标签中设置“defer”属性。 (立即脚本没有被包含在一个function块中,因此会在加载过程中执行。) 设置“defer”属性后，IE就不必等待该脚本装载和执行完毕。这样页面加载会更快。一般来说，这也表明立即脚本最好放在function块中，并在 document或者body对象的onload 句柄中处理该函数。在有一些脚本需要依赖用户操作而执行时----例如点击按钮，或者移动鼠标到某个区域----使用该属性非常有用。但当有一些脚本需要在页面加载过程中或加载完成后执行，使用defer属性得到的好处就不太大。

`script`中的defer属性默认情况下是false的。按照DHTML编程宝典中的描述，对于Defer属性是这样写的：
Using the attribute at design time can improve the download performance of a page because the browser does not need to parse and execute the script and can continue downloading and parsing the page instead.
也就是说：如果是编写脚本的时候加入defer属性，那么浏览器在下载脚本的时候就不必立即对其进行处理，而是继续对页面进行下载和解析，这样会提高下载的性能。
这样的情况有很多种。比如你定义了很多javascript变量，或者在引用文件（.inc）中写了很多的脚本需要处理，那不妨在这些脚本中加入defer属性，对性能的提高肯定有所帮助。
举例如下：
``` bash
<script language="javascript" defer>
var object = new Object();
....
</script>
```

因为defer属性默认是为false的，那么在这里
``` bash
<script language="javascript" defer>
```
显式声明defer属性后等同于
``` bash
<script language="javascript" defer=true>
```
声明了defer属性之后，需要判断是否有别的变量引用了defer脚本块中的变量，否则的话会导致脚本错误的产生。