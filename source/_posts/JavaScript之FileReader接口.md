---
title: JavaScript之FileReader接口
date: 2017-06-06 15:10:24
tags: javascript
---
记得以前做网站时，曾经需要实现一个图片上传到服务器前，先预览的功能。当时用html的file标签一直实现不了，最后舍弃了这个标签，使用了其他方式来实现了这个功能。今天无意发现了一个知识点，用html的file标签就能实现图片上传前预览，感觉很棒，记录一下！就是通过file标签和js的FileReader接口，把选择的图片文件调用readAsDataURL方法，把图片数据转成base64字符串形式显示在页面上。
### 代码实现
``` bash
		<div style="border:2px dashed red;">
            <p>
                图片上传前预览：<input type="file" id="xdaTanFileImg" onchange="xmTanUploadImg(this)"/>
            </p>
            <img id="xmTanImg"/>
            <div id="xmTanDiv"></div>
        </div>
        <hr />
        <script type="text/javascript">            
            //判断浏览器是否支持FileReader接口
            if (typeof FileReader == 'undefined') {
                document.getElementById("xmTanDiv").InnerHTML = "<h1>当前浏览器不支持FileReader接口</h1>";
                //使选择控件不可操作
                document.getElementById("xdaTanFileImg").setAttribute("disabled", "disabled");
            }

            //选择图片
            function xmTanUploadImg(obj) {
                var file = obj.files[0];
                
                console.log(obj);console.log(file);
                console.log("file.size = " + file.size);  //file.size 单位为byte

                var reader = new FileReader();

                //读取文件过程方法
                reader.onloadstart = function (e) {
                    console.log("开始读取....");
                }
                reader.onprogress = function (e) {
                    console.log("正在读取中....");
                }
                reader.onabort = function (e) {
                    console.log("中断读取....");
                }
                reader.onerror = function (e) {
                    console.log("读取异常....");
                }
                reader.onload = function (e) {
                    console.log("成功读取....");

                    var img = document.getElementById("xmTanImg");
                    img.src = e.target.result;
                    //或者 img.src = this.result;  //e.target == this
                }
				//图片数据转成base64字符串形式。
                reader.readAsDataURL(file)
            }
        </script>

```
另外 FileReader除了有函数readAsDataURL，另外还有另外两个函数readAsBinaryString 和 readAsText，分别可以将选择的文件读取成二进制和文本格式 
### 代码实现
``` bash
<script type="text/javascript">
            //判断浏览器是否支持FileReader接口
            if (typeof FileReader == 'undefined') {
                document.getElementById("xmTanContentDiv").InnerHTML = "<p>当前浏览器不支持FileReader接口！</p>";
                document.getElementById("xmTanFile").setAttribute("disabled", "disabled");
            }

            //选择文件
            function xmTanUploadFile(obj){
                if (obj.files.length < 1) return;

                var file = obj.files[0];
                
                if (file.size > 1024 * 1024) {
                    alert("文件大于1M， 太大了，小点吧！");
                    obj.value = "";
                    return;
                }
            }

            //读取文件为二进制
            function readAsBinaryString() {
                var obj = document.getElementById("xmTanFile");
                if (obj.files.length < 1) return;

                var file = obj.files[0];
                var reader = new FileReader();

                //将文件以二进制形式读入页面
                reader.readAsBinaryString(file);
                reader.onload = function (f) {
                    document.getElementById("xmTanContentDiv").innerHTML = this.result;
                }
            }

            //读取文件为文本
            function readAsText() {
                var obj = document.getElementById("xmTanFile");
                if (obj.files.length < 1) return;

                var file = obj.files[0];
                var reader = new FileReader();

                //将文件以文本形式读入页面
                reader.readAsText(file);
                reader.onload = function (f) {
                    document.getElementById("xmTanContentDiv").innerHTML = this.result;
                }
            }
        </script>
        <div style="border: 2px dashed red; padding: 20px 0px;">
            <label>选择文件：</label>
            <input type="file" id="xmTanFile" accept=".html,.js,.css,.txt,.cs,.xml" onchange="xmTanUploadFile(this)"/>
            <input type="button" value="读取成二进制数据" onclick="readAsBinaryString()" />
            <input type="button" value="读取成文本数据" onclick="readAsText()" />
            <input type="button" value="隐藏读取内容" onclick="document.getElementById('xmTanContentDiv').style.display = 'none';"/>
            <input type="button" value="显示读取内容" onclick="document.getElementById('xmTanContentDiv').style.display = 'block';"/>
            <div id="xmTanContentDiv"></div>
        </div>
```