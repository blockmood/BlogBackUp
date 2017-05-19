---
title: vue-roter路由在项目中的使用思路。
date: 2017-05-19 11:10:24
tags: vue
---
之前做了一个项目，做到文章详情页的时候发现可以跳转到文章详情页，但是导航不会随之消失，后来发现这就是我路由没有配置好的问题，现在把这个正确的配置放上来，基本的大部分app都大同小异。

### 正确导航的一个配置  看例子
``` bash
routes: [
    {
      path: '/',
      redirect: { name: 'Nav' }   //默认跳转到导航组件 
    },
    {
      path:'/Nav',			   //导航组件 （网站导航组件）
      name:'Nav',
      component:Nav,
      redirect: { name: 'About' },  //跳转到about组件 （可以看做是文章列表组件）
      children:[
        {
          path:'/Home/:id',	
          name:'Home',
          component:Home
        },
        {
          path:'/About/:id',
          name:'About',
          component:About
        }
      ] 
    },
    {
      path:'/Where',  //文章内容页面组件
      name:'Where',
      component:Where,
    }
  ]
```
### 在导航Nav组件里面的配置
``` bash
{<div class="nav">
	//点击跳转对应的路由组件
  	<router-link :to="{name:'About',params:{id:1}}">Home</router-link>
  	<router-link :to="{name:'About',params:{id:2}}">About</router-link>
	//用于显然的列表
   	<router-view></router-view>
</div>}
```

在文章里列表组件中，可以利用$router.push('组件名')跳转到相应的文章内容组件，并且导航是不会显示的。

至于如何通过不同的ID来切换相同组件的数据，可以看我上一篇文章。