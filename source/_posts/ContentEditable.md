---
title: 浅谈富文本编辑
date: 2017-03-24 16:00:33
tags:
     - javascript
     - contenteditable
     - React
---

最近做的一个demo用到了富文本编辑，

然后我使用React框架来重写这个demo，需要处理富文本编辑功能，就有了此文章

<!--more-->

### 说在前面的话

富文本编辑，又称为WYSIWYG（What You See Is What You Get，所见即所得），

该技术的本质，就是在页面中嵌入一个包含空HTML页面的iframe。

JavaScript实现富文本有两种方式，一种是使用`iframe`元素，另一种是使用`contenteditable`属性，本文讲的是后一种方法。

</br>

### 使用contenteditable属性

可以把`contenteditable`属性应用给页面的任何元素，然后用户立即就可以编辑该元素。这种方法很受欢迎，因为它不需要iframe、空白页和JavaScript，只要为元素设置`contenteditable`属性即可，如下：

```html
<div class="editable" contenteditable=="true"></div>
```

`contenteditable`属性有三个可能的值：“true”表示打开、“false”表示关闭、“inherit”表示可以从父元素那里继承。

</br>

### React中使用

我在重写demo的时候是将可编辑的div做成react控件，以此达到目的。

代码如下：

```javascript
var ContentEditable = React.createClass({
            shouldComponentUpdate: function(nextProps){
                return nextProps.html !== ReactDOM.findDOMNode(this).innerHTML;
            },

            editChange: function(){
                var html = ReactDOM.findDOMNode(this).innerHTML;
                if (this.props.onChange && html !== this.lastHtml) {

                    this.props.onChange({
                        target: {
                            value: html
                        }
                    });
                }
                this.lastHtml = html;
            },

            render: function(){
                return <div className="t_contant"
                    onInput={this.editChange}
                    onBlur={this.editChange}
                    contentEditable="true"
                    dangerouslySetInnerHTML={{__html: 		this.props.html}}></div>;
            }
        });
```

由于`div`标签没有onChange事件，所以需要用onInput和onBlur来代替。

dangerouslySetInnerHTML是React特有的属性，用于提供插入纯 HTML 字符串的功能。

好的，基于以上控件，可编辑的div可用于React中。