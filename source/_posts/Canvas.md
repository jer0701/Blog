---
title: Canvas
date: 2017-03-20 21:34:19
tags:
     - canvas
     - javascript
---

HTML5添加的最受欢迎的功能就是canvas元素

今天我就来学习学习它。

<!--more-->



### 基本用法

与浏览器环境中的其他组件类似，canvas由几组API构成，但非所有的浏览器都支持这些API。除了具备基本绘制能力的2D上下文，还有个WebGL的3D上下文。



要使用canvas元素，必须先设置其width和height属性，指定绘图的区域大小。出现在开始和结束标签中的内容是后备信息，如果浏览器不支持canvas元素，就会显示这些信息。如下

```html
<canvas id="drawing" width="200" height="200">Info</canvas>
```

要在这块画布（canvas）上绘图，需要获得绘图上下文。通过getContext()方法调用,

传入“2d”，就可以获得2D上下文对象,如下

```javascript
var draw = document.getElementById("drawing");
if (draw.getContext) { //判断浏览器是否支持canvas元素
	var context = draw.getContext("2d");
  	...
}
```



</br></br>

</br></br>

### 2D上下文

使用2D绘图上下文提供的方法，可以绘制简单的2D图形，如矩形。2D上下文的坐标开始与canvas元素的左上角，坐标是(0,0)。所有坐标值基于这个点计算。

#### 填充和描边

2D上下文两种基本的操作就是填充和描边。操作的属性是：fillStyle和strokeStyle。

这两个属性的值默认都是“#000000”

```javascript
var context = draw.getContext("2d");
context.strokeStyle = "red";
context.fillStyle = "#0000ff"; //蓝色
```

</br></br>

#### 绘制矩形

```javascript
//红色填充矩形
context.fillStyle = "red";
context.fillRect(10, 10, 50, 50);
```

```javascript
//红色描边矩形
context.strokeStyle = "red";
context.strokeRect(10, 10, 50, 50);
```

```javascript
//在两个矩形重叠的地方清除一个小矩形
context.fillStyle = "red";
context.fillRect(10,10,50,50);
context.fillStyle = "rgba(0, 0, 255, 0.5)";
context.fillRect(30,30,50,50);
context.clearRect(40, 40, 10, 10);
```

</br></br>

#### 绘制路径

要绘制路径，首先必须调用beginPath()方法，表示要开始绘制新路径了。

绘制路有很多方法

```java
arc(x, y, radius, startAngle, endAngle, counterclockwise)
arcTo(x1, y1, x2, y2, radius)
bezierCurveTo(c1x, c1y, c2x, c2y, x, y)//绘制曲线，到x,y为止,途经c1,c2
lineTo(x, y)
moveTo(x, y)//移动光标，不画线
rect(x, y, width, height)
```

看如下例子，绘制不带数字的时钟

```javascript
//开始路径
context.beginPath();
//绘制外圆
context.arc(100, 100, 99, 0, 2*Math.PI, false);
//绘制内圆
context.moveTo(194, 100);
context.arc(100, 100, 94, 0, 2*Math.PI, false);
//绘制分针
context.moveTo(100, 100);
context.lineTo(100, 15);
//绘制时针
context.moveTo(100, 100);
context.lineTo(35, 100);
//描边刚刚画的路径
context.stroke();
```

</br></br>

#### 绘制文本

绘制文本有两个方法：fillText() 和strokeText()。这两个方法都可以接收四个参数：要绘制的字符串、x坐标、y坐标和可选的最大像素宽度。

```javascript
context.font = "blod 14px Microsoft";
context.textAlign = "center";
context.textBaseLine = "middle";
context.fillText("12", 100, 20);
```

</br></br>

#### 变换

变换就是使用不同的变换矩阵处理图像，有如下变换矩阵：

```java
rotate(angle) //围绕原点旋转
scale(scaleX, scaleY) //x,轴缩放
translate(x, y) //将原点坐标移到x,y，即(x,y)代表原点
transform(m1_1, m1_2, m2_1, m2_2, dx, dy) //自定义修改矩阵
setTransform(m1_1, m1_2, m2_1, m2_2, dx, dy) //先把矩阵恢复默认再进行transform
```

</br></br>

#### 绘制图像

使用drawImage()方法就可以把图像绘制到画布上

```java
drawImage(image, x, y) //参数不固定，可以更多
```

</br></br>

#### 阴影

2D上下文会根据以下几个属性的值，自动为形状或路径绘制出阴影

```java
shadowColor: 用CSS颜色格式表示阴影颜色，默认黑色
shadowOffsetX: x轴方向阴影偏移量，默认为0
shadowOffsetY: Y轴方向阴影偏移量，默认为0
shadowBlur: 模糊的像素数，默认为0，即不模糊
```

这些属性可以通过context对象来修改，只要在绘制之前设置合适的值，就能自动产生阴影。例如下：

```javascript
context.shadowOffsetX = 5;
context.shadowOffsetY = 5;
context.Blur = 4;
context.shadowColor = "rgba(0, 0, 0, 0.5)";

context.fillStyle = "red";
context.fillRect(10,10,50,50);
```

</br></br>

#### 渐变

渐变有CanvasGradient实例表示。

Context对象可以通过createLinearGradient()和createRadialGradient()两个方法创建渐变对象，这两个方法的参数如下：

**createLinearGradient(x1, y1, x2, y2);**

创建一个从(x1, y1)点到(x2, y2)点的**线性**渐变对象。

**createRadialGradient(x1, y1, r1, x2, y2, r2);**

创建一个从以(x1, y1)点为圆心、r1为半径的圆到以(x2, y2)点为圆心、r2为半径的圆的**径向**渐变对象。

渐变对象创建完成之后必须使用它的addColorStop()方法来添加颜色，该方法的使用如下：

**addColorStop(position, color);**

其中position表示添加颜色的位置，取值范围为[0, 1]，0表示起点，1表示终点；color表示添加的颜色，取值可以是任何CSS颜色值。

渐变对象创建并配置完成之后就可以将其赋予Context对象的strokeStyle属性或者fillStyle属性，然后绘制的图形就具有了所需的渐变效果。

```javascript
var gradient = context.createLinearGradient(30, 30, 70, 70);
gradient.addColorStop(0, "white");
gradient.addColorStop(1, "black");

context.fillStyle = gradient; //赋值给fillStyle属性
context.fillRect(30, 30, 50, 50);
```

</br></br>

#### 图像数据

2D上下文的一个明显长处就是，可以通过getImageData()获取原始图像数据。这个方法接收四个参数：要获取器数据的画面区域的x、y坐标已经该区域的像素宽度和高度。

```javascript
//取得左上角坐标为(10,5)，大小为50 x 50像素的区域的图像数据
var imageData = context.getImageData(10, 5, 50, 50);
```

上述代码返回的对象是ImageData的实例。每个ImageData对象有三个属性：width、height和data。其中data是一个数组，保存着图像每个像素的数据，分别是四个元素：红、绿、蓝和透明度值。例如下：

```javascript
var data = imageData.data,
    red = data[0],
    green = data[1],
    blue = data[2],
    alpha = data[3];
```

</br></br>

</br></br>



好了，到这里对canvas 2D上下文的基本用法就有了一定的了解，另外还有两个有意思的属性：globalAlpha和globalCompositionOperation，可去测试一下。

