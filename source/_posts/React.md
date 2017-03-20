---
title: React
date: 2017-03-20 19:00:13
tags:
     - React	
     - React-Router
---

最近在学习React框架，这是前端最热门框架之一了。

碰到过一些坑，在这里记录下来。

<!--more-->

### React入门

React是Facebook开源的项目，是一套性能比较好的框架。为什么呢，因为采取虚拟DOM技术，通过对DOM的模拟，最大限度地减少与DOM的交互。

React代码逻辑比较简单，如果不会的推荐可以看一看阮一峰老师的[入门教程](http://www.ruanyifeng.com/blog/2015/03/react.html)



### React Router

接下来我讲讲碰到的坑了，没错，就是react路由的坑

react router上手也容易，大家可以去看阮一峰老师的[教程](http://www.ruanyifeng.com/blog/2016/05/react_router.html?utm_source=tool.lu)



我在代码中是这样使用的

```javascript
import {Router, Route, IndexRoute, hashHistory} from 'react-router'
...
ReactDOM.render(
     (<Router history={hashHistory}>
        <Route path='/' component={App} >
            <IndexRoute component={Gallery}/>
            <Route path='/calculator' component={Calculator} />
        </Route>
    </Router>),
    document.getElementById('app')
);
```

因为是本地静态页面，所以使用hashHistory。

项目采用yoman脚手架搭建，react router安装是使用如下命令

```bash
npm install -S react-router
```

然后npm start运行项目，发现报错了，如下

```bash
Uncaught TypeError: Cannot read property 'location' of undefined
```

location未定义

可是location是history的呀如下

```java
match:r.computeMatch(r.props.history.location.pathname)
```

history等于hashHistory，react-router定义的，怎么会报错呢，郁闷呀。

开始以为是我的代码写错了，然后就去写了个demo，如下

```htm
<!DOCTYPE html>
<html>
  <head>
    <script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
    <script src="../build/react-router.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
		var Link = ReactRouter.Link;
 	var IndexLink = ReactRouter.IndexLink;
 	var Router = ReactRouter.Router;
 	var Route = ReactRouter.Route;
 	var IndexRoute = ReactRouter.IndexRoute;
 	var hashHistory = ReactRouter.hashHistory;
	
	var App = React.createClass({
 			render : function(){
 				return (
 					<div className="container">
					Hello world!
 					</div>
 				)
 			}
 		})

      ReactDOM.render(
        <Router history={hashHistory}>
 			    <Route path="/" component={App}>
 			     
 			    </Route>
 		    </Router>,
        document.getElementById('example')
      );
    </script>
  </body>
</html>

```

测试发现是OK的，页面显示正常。

但是修改一下：

```html
<script src="../build/react-router.min.js"></script>  修改成
<script src="http://cdn.bootcss.com/react-router/4.0.0/react-router.min.js"></script>
```

然后在测试，发现报一样的错了。

原来我项目中安装react-router是安装最新的4.0的版本

```javascript
"dependencies": {
    "core-js": "^2.0.0",
    "normalize.css": "^4.0.0",
    "react": "^15.0.0",
    "react-addons-css-transition-group": "^15.4.2",
    "react-dom": "^15.0.0",
    "react-router": "^4.0.0"
  }
```

手动改为"^3.0.0"再npm install生成就OK了。

4.0版本改动比较大，大家可以看下[这篇文章](https://zhuanlan.zhihu.com/p/22490775)

好了，今天就记录到这吧.