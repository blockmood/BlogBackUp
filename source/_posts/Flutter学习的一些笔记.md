---
title: Flutter学习的一些笔记
date: 2019-07-02 11:10:24
tags: Flutter
---

具体api参照官网。

## 布局(部件)

> Row 横向布局
> Column 纵向布局

## 事件

> 点击

```
InkWell
```

## 列表

> GridView.count 

```
常用9宫格方式布局
```

> ListView.builder

```
常用上下列表布局
```

## 网络布局

> FutureBuilder(...)

案列

```
body:SingleChildScrollView(
	child:FutureBuilder(
		future: //远程请求方法
		builder:(context,snapshot//当前数据){
			return Column(
				chidren:[
					...
				]
			)
		}
	)
)
```

## BUG

> 1.高度超过指定设备报错解决方案

```
外层套 SingleChildScrollView
```

