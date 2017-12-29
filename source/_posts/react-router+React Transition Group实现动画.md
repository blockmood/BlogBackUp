---
title: react-router+React Transition Group实现动画
date: 2017-12-27 15:10:24
tags: react-router
---

```
import React from 'react'
import {Route,Switch} from 'react-router-dom'
import Home from './Home'
import About from './About'
import {TransitionGroup,CSSTransition } from 'react-transition-group'
import './index1.css'

const RouterMap = () => (
    <Route render={({location})=>
        <TransitionGroup>
            <CSSTransition key={location.pathname.split('/')[1]} timeout={{enter:300,exit:10}} 
                classNames={`fade-${location.pathname ? location.pathname.split('/')[1] : ''}`} 
                mountOnEnter={false} 
                unmountOnExit={false}
            >
                <Switch location={location}>
                    <Route exact path="/" component={Home} />
                    <Route path="/about" component={About} />
                </Switch>
            </CSSTransition>
        </TransitionGroup>
    }/>
)

export default RouterMap

```

[详细文档](https://reactcommunity.org/react-transition-group/)