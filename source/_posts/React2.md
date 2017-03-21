---
title: React之通信
date: 2017-03-21 19:37:23
tags:
     - React
---

今天这篇文章的目的是总结我今天做项目碰到的问题

方便以后查阅

<!--more-->

### 需求

先描述一下需求，我想在父组件（Heart）中添加子组件（Shadow），且子组件有很多个。子组件（Shadow）其他内容都相同，就是Color不同，所以只要在Shadow render的时候添加不同的颜色样式就可以了。

</br></br>

#### 问题一

在父组件中有个button，每点击一次产生一个阴影即添加一个子组件。

怎么控制呢？



在父组件中添加一个state：shadows[]数组, 数组保存的是每一个子组件

```javascript
getInitialState: function() {
     return { shadows: [] };
}
```

当执行点击函数时，我们就push一个子组件进去，这样就可以点击一次添加一个子组件了。

```javascript
handleClick: function(event) {
     var temp = this.state.shadows;
     temp.push(<Shadow />);
     this.setState({shadows: temp});
}
```

</br></br>

</br></br>

#### 问题二

解决问题一之后就会出现第二个问题，就是每添加一个子组件，都会产生一个DOM节点，不断的点击，就会保存很多个相同的DOM节点，因为每个子组件出现的效果只会显示很短的时间，效果没了之后，这些节点没有存在的必要了，所以有必要在效果结束之后删除这些节点。

问题来了，那么怎么删除呢？

在这一步我卡了较久的时间，最开始想通过子组件自己删除自己，没有实现。。。

最后发现，可以根据问题一的解决来实现，通过父组件来删除子组件！

在问题一种，我们添加了一个state：shadows[]数组，那么我每次从数组中pop出或者shift出一个元素，那不就可以了么。

```javascript
clearShadow: function() {
      return function() {
           var temp = this.state.shadows;
           temp.shift();
           this.setState({shadows: temp});
     }.bind(this);
}
```

</br></br>

接下来是什么时候删除呢，当然是效果完成之后，也就是子组件render之后。

所以我在子组件的componentDidMount函数中调用父组件的清除函数

```javascript
componentDidMount: function() {
     setTimeout(function() {
         this.props.clearShadow();
     }.bind(this), 10000);
}
```



为什么在componentDidMount函数中调用呢，这就要谈及到组件的生命周期了。

这个可以查看[官方文档](https://facebook.github.io/react/docs/react-component.html)


我在这里提一下生命周期提供的API:

1. getDefaultProps

   作用于组件类，只调用一次，返回对象用于设置默认的`props`，对于引用值，会在实例中共享

2. getInitialState

   作用于组件的实例，在实例创建时调用一次，用于初始化每个实例的`state`，此时可以访问`this.props`

3. componentWillMount

   在完成首次渲染之前调用，此时仍可以修改组件的state

4. render

   必选的方法，创建虚拟DOM

5. componentDidMount

   真实的DOM被渲染出来后调用，在该方法中可访问到真实的DOM元素。此时已可以使用其他类库来操作这个DOM

6. componentWillReceiveProps

   组件接收到新的`props`时调用，并将其作为参数`nextProps`使用，此时可以更改组件`props`及`state`

7. shouldComponentUpdate

   组件是否应当渲染新的`props`或`state`，返回`false`表示跳过后续的生命周期方法，通常不需要使用以避免出现bug。在出现应用的瓶颈时，可通过该方法进行适当的优化

8. componentWillUpdate

   接收到新的`props`或者`state`后，进行渲染之前调用，此时不允许更新`props`或`state`

9. componentDidUpdate

   完成渲染新的`props`或者`state`后调用，此时可以访问到新的DOM元素

10. componentWillUnmount

    组件被移除之前被调用，可以用于做一些清理工作，在`componentDidMount`方法中添加的所有任务都需要在该方法中撤销，比如创建的定时器或添加的事件监听器



</br></br>

</br></br>

博客标题是React之通信，今天说的就是父组件和子组件之间通信，有如下两个方法：

父组件调用子组件的方法：使用ref属性，子组件是一个DOM节点，可以通过`this.refs.[refName]` 就会返回这个真实的 DOM 节点。

子组件调用父组件方法：使用props属性，就是我今天使用的方法。

```javascript
temp.push(<Shadow clearShadow={this.clearShadow()}/>);
```

然后就可以通过`this.props.clearShadow()`来调用了。



完美~