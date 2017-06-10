title: react-router v4
date: 2017-06-08 22:55:42
tags: React
banner:
---
React 成了当前前端的主流开发框架, 但 React 只是作为 view 展示层, 要想进行完整的App开发, 需要配合页面路由系统, 及状态管理解决方案. 组合在一起就是React生态中最常见的: React+React-router+Redux 组合. React-router 依靠位置栏上的请求 URL 和浏览器的操作历史来渲染不同的页面组件. 本文主要介绍React-router 的最新版本V4.
<!-- more -->

### V2 基本使用
Router 之前主流的版本为v2和v3, 其主要使用方法为通过 Router 和 Route 声明路径和组件的映射关系, 并且通过 history 管理访问历史和路由切换.其基本使用方法如下:

```jsx
import {browserHistory, Router, Route} from 'react-router';
React.render(
    <Router history={browserHistory}>
        <Route path='/' component={Layout} onEnter={requireAuth}>
            <Route path='/demo' component={Demo} />
        </Route>
        <Route path='/login' component={Login}/>
    </Router>
)
```
具体使用方法可参看阮一峰大神的[React router使用教程](http://www.ruanyifeng.com/blog/2016/05/react_router.html), 有很详细的介绍.

### V4 介绍

#### 基本介绍和使用
V4 版的 router 进行了代码完全重写, 其遵循 React 的 Just Component;声明式;可组合性的设计理念, 所有的东西都是一个组件: Router, Link, Switch. 而且其代码库进行了拆分, 拆分成五个库:

* react-router React Router 核心
* react-router-dom 用于 DOM 绑定的 React Router
* react-router-native 用于 React Native 的 React Router
* react-router-redux React Router 和 Redux 的集成
* react-router-config 静态路由配置帮助助手

React-router 是核心基础库, router-dom 则用于 web 开发, router-native 用于React native 开发. 在 v4 中有两种基本的方法指定 url 对应的组件.
一是component, 二是render.

```js
import {BrowserRouter as Router, Route, Link, NavLink, Switch, Redirect} from 'react-router-dom';
const BasicExample = () => (
  <Router>
    <div>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
        <li><Link to="/topics">Topics</Link></li>
      </ul>
      <hr/>
      <Route exact path="/" component={Home}/>
      <Route path="/about" component={About}/>
      <Route path="/topics" render={() => (<h3>Hello world</h3>)}/>
    </div>
  </Router>
)
```

#### Auth
Auth check 是 web 开发的常见需求, 检查用户是否登录, 若未登录跳转到登录页. 在 v2 版本中提供了路由切换的事件 hook, 在 hook 中做判断. 在 v4 中则需要在组件 render 的时候判断, 如果未登录使用 Redirect 组件跳转到登录页

```jsx
// 基本用法
<Route render={() => {
  isAuthenticated ? <Component /> : <Redirect to={{path: '/login', state: {from: props.location}}} />
}} />
// 通常多个路由需要 Auth check, 这样写会比较繁琐, 可以进行组件封装
const PrivateRoute = ({ component: Component, ...rest }) => (
  <Route {...rest} render={props => (
    fakeAuth.isAuthenticated ? (
      <Component {...props}/>
    ) : (
      <Redirect to={{
        pathname: '/login',
        state: { from: props.location }
      }}/>
    )
  )}/>
)
```

#### 路由切换
手动路由切换也是常见的开发需求, 在 v2 中可以通过引入 history 库的对应对象, 通过 push 或 replaceState 来实现. 在 v4 中如果你还这样使用会发现 url 切换了, 但页面显示内容并没有变化, 细心的朋友可能会发现在 v4 中路由声明时不需要传入 history 对象了, v4在内部实例化了自己的 history 对象. 如果想要获取到 history 对象, 则需要使用 withRouter 方法

```jsx
import React from 'react'
import PropTypes from 'prop-types'
import { withRouter } from 'react-router'

// A simple component that shows the pathname of the current location
class ShowTheLocation extends React.Component {
  static propTypes = {
    match: PropTypes.object.isRequired,
    location: PropTypes.object.isRequired,
    history: PropTypes.object.isRequired
  }
  render() {
    const { match, location, history } = this.props

    return (
      <div>You are now at {location.pathname}</div>
    )
  }
}
// Create a new component that is "connected" (to borrow redux
// terminology) to the router.
const ShowTheLocationWithRouter = withRouter(ShowTheLocation)
```

#### 多 Layout
实际开发过程中, 多个页面会有相同的 layout, 在基本使用示例中的代码即实现了layout, 但有时两个页面可能 layout 不同, 这时跟 auth check 差不多还可以通过 render 或封装组件的方式实现.

```jsx
// 通过 render 实现
<Route render={() => {return <Layout1><Component /></Layout1>}} />
<Route render={() => {return <Layout2><Component /></Layout2>}} />
// 组件封装
const RouteWithLayout1 = ({ component: Component, ...rest }) => (
  <Route {...rest} render={props => (
    <Layout1><Component/></Layout1>
  )}/>
)
const RouteWithLayout2 = ({ component: Component, ...rest }) => (
  <Route {...rest} render={props => (
    <Layout2><Component/></Layout2>
  )}/>
)
```
这种方法有一个缺点是Layout组件在路由切换的时候每次都会重新渲染. 目前还没发现解决办法.

有一篇博客很详细的解释了[多Layout实现](https://simonsmith.io/reusing-layouts-in-react-router-4/)

#### 文档

* [官方文档](https://reacttraining.com/react-router/web/guides/quick-start)



