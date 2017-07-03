---
title: javascript时间戳和日期字符串相互转换
date: 2017-07-03 14:12:56
tags: javascript
---
``` bash
// 获取当前时间戳(以s为单位)
var timestamp = Date.parse(new Date());
timestamp = timestamp / 1000;
//当前时间戳为：1403149534
console.log("当前时间戳为：" + timestamp);
```
``` bash
// 获取某个时间格式的时间戳
var stringTime = "2014-07-10 10:21:12";
var timestamp2 = Date.parse(new Date(stringTime));
timestamp2 = timestamp2 / 1000;
//2014-07-10 10:21:12的时间戳为：1404958872 
console.log(stringTime + "的时间戳为：" + timestamp2);
```

### 时间戳转日期格式
``` bash
<script>     
function getLocalTime(nS) {     
   return new Date(parseInt(nS) * 1000).toLocaleString().replace(/:\d{1,2}$/,' ');     
}     
alert(getLocalTime(1293072805));     
</script> 
```
结果是 2010-12-23 10:53
``` bash
 <script>     
    function getLocalTime(nS) {     
       return new Date(parseInt(nS) * 1000).toLocaleString().replace(/年|月/g, "-").replace(/日/g, " ");      
    }     
    alert(getLocalTime(1177824835));     
</script>  
```
结果是 2010年12月23日 10:53

``` bash
console.log(Date.parse(new Date('2017-07-03 23:59:59')) / 1000);	//转为时间戳
console.log(new Date(parseInt(1499097599) * 1000).toLocaleString());	//转为日期
```