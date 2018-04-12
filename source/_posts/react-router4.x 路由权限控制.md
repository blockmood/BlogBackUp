---
title: react-router4.x 路由权限控制
date: 2018-04-10 19:10:24
tags: react-router
---

想必大家在使用react-router的时候，有的组件是需要登录才能访问的（private），有的是开放的（public）

现在给大家介绍一下我的实现方式（仅供参考，如有更好的思路，欢迎在评论区回复）

## 1.第一种封装一个私有路由

这是我的路由配置，Route大家都知道。不用多讲。PrivateRoute 这个是我自己封装的代码如下：
PrivateRoute.jsx

```
import React from 'react';
import {Route,Redirect,withRouter} from 'react-router-dom';
import PropTypes from 'prop-types';
import storage from 'utils/storage.js';
//私有路由，只有登录的用户才能访问
class PrivateRoute extends React.Component{
    componentWillMount(){
        let  isAuthenticated =  storage.getItem("token") ? true :false;
        this.setState({isAuthenticated:isAuthenticated})
        if(!isAuthenticated){
          const {history} = this.props;
          setTimeout(() => {
            history.replace("/login");
          }, 1000)
        }
    }
    render(){
        let { component: Component,path="/",exact=false,strict=false} = this.props;
        return this.state.isAuthenticated ?  (
            <Route  path={path} exact={exact}  strict={strict}  render={(props)=>( <Component {...props} /> )} />
        ) : ("请重新登录");
    }
}
PrivateRoute.propTypes  ={
        path:PropTypes.string.isRequired,
        exact:PropTypes.bool,
        strict:PropTypes.bool,
        component:PropTypes.func.isRequired
}
export default withRouter(PrivateRoute);
```
使用：
```
.......
import React from 'react';
import {BrowserRouter as Router , HashRouter ,  Route,Link,Switch ,withRouter} from 'react-router-dom';
import {syncHistoryWithStore} from 'react-router-redux'
import reducer from '../reducers/index.js';
import Home from 'container/Home.jsx';
import Login from 'container/Login.jsx';
import Regist from 'container/Regist';
import PrivateRoute from 'component/common/PrivateRoute.jsx';
.......
<Route {...props} path="/login" component={Login} />
<Route {...props} path="/regist" component={Regist} />
<PrivateRoute  path="/home"  component={Home} />
<Route  {...props}  component={Login} />

```
代码很简单，仔细看一下就明白。这里的PrivateRoute 属性接口和 Route基本类似。我这里只是提供一种思路，大家可以根据自己的业务添加自己的特色。


## 2.第二种是使用高阶组件。

> 高阶组件（HOC）是react中对组件逻辑进行重用的高级技术。但高阶组件本身并不是React API。它只是一种模式，这种模式是由react自身的组合性质必然产生的。具体而言，高阶组件就是一个函数，且该函数接受一个组件作为参数，并返回一个新的组件。

现在知道了高阶组件是什么，现在我们就用该思路来做一个高阶私有路由 HocPrivateRoute代码如下
HocPrivateRoute.jsx

```
import React from 'react';
import {Route,Redirect,withRouter} from 'react-router-dom';
import PropTypes from 'prop-types';
import storage from 'utils/storage.js';

function withHocPrivateRoute(WrappedComponent,hocProps){
    if(!!!WrappedComponent){
        throw new Error("缺少组件参数");
        return false;
    }
    //withRouter 也是一个高阶组件 传递 history
    return withRouter(
    class extends React.Component{
        constructor(props) {
          super(props);
        }

        componentWillMount(){
            let  isAuthenticated =  storage.getItem("token") ? true :false;
            this.setState({isAuthenticated:isAuthenticated})
            if(!isAuthenticated){
              const {history} = this.props;
              setTimeout(() => {
                history.replace("/login");
              }, 1000)
            }
        }

        render(){
            return this.state.isAuthenticated ?  (
                <WrappedComponent {...hocProps} />
            ) : ("请重新登录");
        }
    }
    )
}


export default withHocPrivateRoute;

```

使用：

```
...
import React from 'react';
import {BrowserRouter as Router , HashRouter ,  Route,Link,Switch ,withRouter} from 'react-router-dom';
import {syncHistoryWithStore} from 'react-router-redux'
import reducer from '../reducers/index.js';
import Home from 'container/Home.jsx';
import Login from 'container/Login.jsx';
import Regist from 'container/Regist';
//import PrivateRoute from 'component/common/PrivateRoute.jsx';
import HocPrivateRoute from 'component/common/HocPrivateRoute.jsx';
const  PrivateRoute =  HocPrivateRoute(Route);
...
<Switch>
<Route {...props} path="/login" component={Login} />
<Route {...props} path="/regist" component={Regist} />
<PrivateRoute  path="/home"  component={Home} />
<Route  {...props}  component={Login} />
</Switch>

```