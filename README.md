# canvas

- Canvas API 提供了一个通过 JavaScript 和 HTML 的 canvas 元素来绘制图形的方式。它可以用于动画、游戏画面、数据可视化、图片编辑以及实时视频处理等方面。

## 1 基本使用

```HTML
<canvas id="canvasNode" width="500" height="500">
  你的浏览器不支持canvas 请升级浏览器
</canvas>

<script>
  /** @type {HTMLCanvasElement} */
  const node = document.getElementById('canvasNode')
  // 获取画布对象
  const ctx = node.getContext('2d')
  // 设置颜色
  ctx.fillStyle = 'green'
  // 绘制矩形
  ctx.fillRect(10, 10, 100, 200)

  console.log(ctx)
</script>
```

- 通过 canvas 标签中的 width 和 height 设置画布的宽高
- canvas.getContext() 方法获取画布的空间 在其上绘制图形

## 2 canvas 动画思想

- canvas 绘制了一个图形,一旦绘制成功了, canvas 没有能力,从画布上再次得到这个图形,也就是说我们没有能力去修改已经在画布上的内容

* **清屏，更新，渲染** 就可以为 canvas 的动画思想

```js
/** @type {HTMLCanvasElement} */
const node = document.getElementById('canvasNode')
// 获取画布对象
const ctx = node.getContext('2d')

let x = 10

setInterval(() => {
  // 清屏
  ctx.clearRect(0, 0, 500, 500)
  // 更新 x坐标
  x += 50
  // 渲染
  ctx.fillStyle = 'blue'
  ctx.fillRect(x, 10, 50, 50)
}, 100)
```

### 面向对象实现 canvas 动画

```js
/** @type {HTMLCanvasElement} */
const node = document.getElementById('canvasNode')
// 获取画布对象
const ctx = node.getContext('2d')

class Rect {
  constructor(x, y, width, height, color, ctx) {
    this.x = x
    this.y = y
    this.width = width
    this.height = height
    this.color = color
    this.ctx = ctx
  }

  render() {
    this.ctx.fillStyle = this.color
    this.ctx.fillRect(this.x, this.y, this.width, this.height)
  }

  update() {
    this.x++
  }
}

const rect = new Rect(0, 10, 50, 50, 'green', ctx)
const rect2 = new Rect(0, 80, 50, 50, 'red', ctx)

setInterval(() => {
  // 清屏
  ctx.clearRect(0, 0, 500, 500)
  // 更新
  rect2.update()
  rect.update()
  // 渲染
  rect2.render()
  rect.render()
}, 30)
```

## 矩形

| 方法/属性                    | 描述                                           |
| ---------------------------- | ---------------------------------------------- |
| fillStyle                    | 设置填充颜色                                   |
| fillRect(x,y,width,height)   | 方法绘制“已填色”的矩形。默认的填充颜色是黑色。 |
| strokeStyle                  | 设置边框颜色                                   |
| strokeRect(x,y,width,height) | 方法绘制矩形边框。默认的填充颜色是黑色。       |
| clearRect(x,y,width,height)  | 清除画布内容                                   |
| globalAlpha                  | 设置透明度 0-1                                 |

```js
const canvas = document.getElementById('canvasNode')
// 获取画布对象
const ctx = canvas.getContext('2d')
// 设置颜色
ctx.fillStyle = 'green'
// 绘制矩形
ctx.fillRect(10, 10, 50, 50)

// 设置边框颜色
ctx.strokeStyle = 'red'
// 绘制边框矩形
ctx.strokeRect(80, 10, 50, 50)
```

![image-20210307171933271](https://gitee.com/MellowCo/BlobImg/raw/master/image-20210307171933271.png)

## 线

| 方法/属性                                                    | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| lineTo(x,y)                                                  | 设置路径坐标                                                 |
| [`lineWidth = value`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineWidth) | 设置线条宽度 大于0的数字。                                   |
| [`lineCap = type`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineCap) | 设置线条末端样式<br>**butt** 方形<br/>**round** 圆形<br/>**square** 末端以方形结束，但是增加了一个宽度和线段相同，高度是线段厚度一半的矩形区域<br/> |
| [`lineJoin = type`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineJoin) | 设定线条与线条间接合处的样式<br>**round** 圆形<br/>**bevel** 平角<br/>**miter** 直角菱形 这个设置可以通过 [`miterLimit`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/miterLimit) 属性看到效果<br/> |
| [`miterLimit = value`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/miterLimit) | 斜接面限制比例的的数字  大于0的数字                          |
| [`getLineDash()`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/getLineDash) | 返回一个包含当前虚线样式，长度为非负偶数的数组               |
| [`setLineDash(segments)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/setLineDash) | 设置当前虚线样式 [宽度,间隔,宽度,间隔]                       |
| [`lineDashOffset = value`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineDashOffset) | 设置虚线样式的起始偏移量                                     |
|                                                              |                                                              |

```js
/** @type {HTMLCanvasElement} */
const canvas = document.getElementById('canvasNode')
// 获取画布对象
const ctx = canvas.getContext('2d')
ctx.lineTo(50, 180)
ctx.lineTo(200, 100)
ctx.lineTo(400, 200)
ctx.lineTo(200, 200)
ctx.strokeStyle = 'red'
ctx.stroke()
```

![image-20210307182927170](https://gitee.com/MellowCo/BlobImg/raw/master/image-20210307182927170.png)

* lineCap 
  * butt
  * round
  * square 

![img](data:image/png;base64,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)

* lineJoin
  * round
  * bevel
  * miter

![img](data:image/png;base64,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)

* setLineDash(segments)

```js
function drawDashedLine(pattern) {
  ctx.beginPath();
  ctx.setLineDash(pattern);
  ctx.moveTo(0, y);
  ctx.lineTo(300, y);
  ctx.stroke();
  y += 20;
}

drawDashedLine([]);
drawDashedLine([1, 1]);
drawDashedLine([10, 10]);
drawDashedLine([20, 5]);
drawDashedLine([15, 3, 3, 3]);
drawDashedLine([20, 3, 3, 3, 3, 3, 3, 3]);
drawDashedLine([12, 3, 3]);
```



![img](data:image/png;base64,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)



## 圆弧

| 方法/属性                                              | 描述                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| arc(x, y, radius, startAngle, endAngle, anticlockwise) | x，y 圆心坐标<br>radius 半径<br>startAngle,endAngle参数用弧度定义了开始以及结束的弧度。这些都是以x轴为基准<br>anticlockwise 为true时，是逆时针方向，否则顺时针方向 |

**注意：`arc()`函数中表示角的单位是弧度，不是角度。角度与弧度的js表达式:**

**弧度=(Math.PI/180)\*角度**

```js
/** @type {HTMLCanvasElement} */
const canvas = document.getElementById('canvasNode')
// 获取画布对象
const ctx = canvas.getContext('2d')
// 圆周长 2π 2*3.14≈7 2*Math.PI
// startAngle endAngle 相差为7时 为一个圆
ctx.arc(100, 100, 50, 5, 7, false)
ctx.strokeStyle = 'red'
ctx.stroke()
```

![image-20210307183159545](https://gitee.com/MellowCo/BlobImg/raw/master/image-20210307183159545.png)

## 路径

1. 首先，你需要创建路径起始点。
2. 然后你使用[画图命令](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D#Paths)去画出路径。
3. 之后你把路径封闭。
4. 一旦路径生成，你就能通过描边或填充路径区域来渲染图形。

| 方法/属性   | 描述                                 |
| ----------- | ------------------------------------ |
| beginPath() | 新建一条路径                         |
| moveTo(x,y) | 设置起始点坐标                       |
| lineTo(x,y) | 设置路径坐标                         |
| closePath() | 闭合路径                             |
| stroke()    | 通过线条来绘制图形轮廓               |
| fill()      | 通过填充路径的内容区域生成实心的图形 |

```js
/** @type {HTMLCanvasElement} */
const canvas = document.getElementById('canvasNode')
// 获取画布对象
const ctx = canvas.getContext('2d')
// 创建路径
ctx.beginPath()
// 设置起始点
ctx.moveTo(100, 100)
// 设置路径
ctx.lineTo(200, 300)
ctx.lineTo(300, 230)
ctx.lineTo(440, 290)
ctx.lineTo(380, 50)
// 封闭路径
ctx.closePath()
// 绘制图形
ctx.strokeStyle = 'red'
ctx.stroke()
// 填充图形
ctx.fillStyle = 'skyblue'
ctx.fill()
```

![绘制路径图](https://gitee.com/MellowCo/BlobImg/raw/master/%E7%BB%98%E5%88%B6%E8%B7%AF%E5%BE%84%E5%9B%BE.png)

## 文字

| 方法/属性                                                    | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`font = value`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/font) | 默认的字体是 `10px sans-serif`。                             |
| [`textAlign = value`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/textAlign) | 文本对齐选项，`start`, `end`, `left`, `right` or `center`. 默认值是 `start`。 |
| [`textBaseline = value`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/textBaseline) | 基线对齐选项. 可选的值包括：`top`, `hanging`, `middle`, `alphabetic`, `ideographic`, `bottom`。默认值是 `alphabetic。` |
| [`direction = value`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/direction) | 文本方向。可能的值包括：`ltr`, `rtl`, `inherit`。默认值是 `inherit。` |
| [`fillText(text, x, y [, maxWidth\])`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/fillText) | 在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的.     |
| [`strokeText(text, x, y [, maxWidth\])`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/strokeText) | 在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的        |

```js
/** @type {HTMLCanvasElement} */
const canvas = document.getElementById('canvasNode')
// 获取画布对象
const ctx = canvas.getContext('2d')
ctx.font = '50px serif'
ctx.fillText('Hello World', 100, 200)

ctx.font = '40px serif'
ctx.strokeText('Hello World', 100, 300)
```

![image-20210307201355822](https://gitee.com/MellowCo/BlobImg/raw/master/image-20210307201355822.png)

## 渐变

| 方法/属性                                                    | 描述                                                         |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| [`createLinearGradient(x1, y1, x2, y2)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/font) | 表示渐变的起点 (x1,y1) 与终点 (x2,y2)                        |
| [`createRadialGradient(x1, y1, r1, x2, y2, r2)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/textAlign) | 前三个定义一个以 (x1,y1) 为原点，半径为 r1 的圆，后三个参数则定义另一个以 (x2,y2) 为原点，半径为 r2 的圆 |
| [`gradient.addColorStop(position, color)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/textBaseline) | `position` 参数必须是一个 0.0 与 1.0 之间的数值，表示渐变中颜色所在的相对位置。例如，0.5 表示颜色会出现在正中间<br>`color` 参数必须是一个有效的 CSS 颜色值（如 #FFF， rgba(0,0,0,1)，等等）。 |

```js
/** @type {HTMLCanvasElement} */
const canvas = document.getElementById('canvasNode')
// 获取画布对象
const ctx = canvas.getContext('2d')
// 线性渐变
const line = ctx.createLinearGradient(10, 10, 50, 50)
line.addColorStop(0, 'blue')
line.addColorStop(1, 'red')
ctx.fillStyle = line
ctx.fillRect(0, 0, 100, 100)
// 渐变圆
const c = ctx.createRadialGradient(100, 210, 50, 100, 250, 100)
c.addColorStop(0, '#A7D30C')
c.addColorStop(0.9, '#019F62')
c.addColorStop(1, 'rgba(1,159,98,0)')
ctx.fillStyle = c
ctx.fillRect(0, 100, 250, 250)
```

![image-20210307202911068](https://gitee.com/MellowCo/BlobImg/raw/master/image-20210307202911068.png)

## 阴影

| 方法/属性                                                    | 描述                 |
| :----------------------------------------------------------- | -------------------- |
| [`shadowOffsetX = float`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/font) | 设定阴影在 X延伸距离 |
| [`shadowOffsetY = float`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowOffsetY) | Y 轴的延伸距离       |
| [`shadowBlur = float`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/textBaseline) | 设定阴影的模糊程度   |
| [`shadowColor = color`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowColor) | 设定阴影颜色效果·    |

```js
/** @type {HTMLCanvasElement} */
const canvas = document.getElementById('canvasNode')
// 获取画布对象
const ctx = canvas.getContext('2d')
ctx.shadowOffsetX = 10
ctx.shadowOffsetY = 20
ctx.shadowBlur = 3
ctx.shadowColor = 'red'

ctx.fillStyle = 'blue'
ctx.fillRect(0, 0, 100, 100)

ctx.font = '50px serif'
ctx.fillStyle = 'green'
ctx.fillText('hello world', 0, 200)
```

![image-20210307203414756](https://gitee.com/MellowCo/BlobImg/raw/master/image-20210307203414756.png)

## 图片

| 方法/属性                                                    | 描述                                                         |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| [`drawImage(image, x, y, width, height)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/drawImage) | x,y起始坐标，width` 和 `height图片大小                       |
| [`drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/drawImage) | 前4个参数指的是你在图片中设置切片的宽度和高度,以及切片位置<br>后4个参数指的是切片在画布上的位置和切片的宽度和高度 |

```js
 // 创建image元素
var img=new Image();
// 设置src地址
img.src="https://tva3.sinaimg.cn/large/006djwNZgy1gmqxcekdm8j31hc0u0dln.jpg"
// 必须在onload 之后绘制图片
img.onload=function(){
    ctx.drawImage(img,100,100,200,100)
}
```

![kfdjhgfkd9856745](https://gitee.com/MellowCo/BlobImg/raw/master/kfdjhgfkd9856745.png)



![image-20210213111523157](https://liu-markdown-img.oss-cn-hangzhou.aliyuncs.com/img/image-20210213111523157.png)



## 变形

### 状态保存 恢复

| 方法/属性                                                    | 描述                                                         |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| [`save()`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/save) | 保存画布(canvas)的所有状态                                   |
| [`restore()`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/restore) | save 和 restore 方法是用来保存和恢复 canvas 状态的，都没有参数。Canvas 的状态就是当前画面应用的所有样式和变形的一个快照。 |

* save 会将状态保存在栈中，先进后出
* 保存属性 [`strokeStyle`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/strokeStyle), [`fillStyle`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/fillStyle), [`globalAlpha`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/globalAlpha), [`lineWidth`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineWidth), [`lineCap`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineCap), [`lineJoin`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineJoin), [`miterLimit`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/miterLimit), [`lineDashOffset`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/lineDashOffset), [`shadowOffsetX`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowOffsetX), [`shadowOffsetY`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowOffsetY), [`shadowBlur`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowBlur), [`shadowColor`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/shadowColor), [`globalCompositeOperation`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/globalCompositeOperation), [`font`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/font), [`textAlign`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/textAlign), [`textBaseline`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/textBaseline), [`direction`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/direction), [`imageSmoothingEnabled`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/imageSmoothingEnabled)
* 移动，旋转和缩放

| 方法/属性                                                    | 描述                                                         |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| [`translate(x, y)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/save) | x 是左右偏移量，y 是上下偏移量                               |
| [`rotate(angle)`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/restore) | 旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值。<br>旋转的中心点始终是 **canvas 的原点**，如果要改变它，我们需要用到 `translate `方法 |
| **scale(x, y)**                                              | `scale ` 方法可以缩放画布的水平和垂直的单位                  |

```js
const canvas = document.getElementById('canvasNode')
const ctx = canvas.getContext('2d')
ctx.save()
ctx.fillStyle = 'blue'
ctx.fillRect(0, 0, 50, 50)

ctx.translate(60, 0)
ctx.fillRect(0, 0, 50, 50)

ctx.translate(60, 0)
ctx.fillRect(0, 0, 50, 50)

ctx.translate(60, 0)
ctx.fillRect(0, 0, 50, 50)

ctx.translate(60, 0)
ctx.fillRect(0, 0, 50, 50)

ctx.restore()
ctx.fillStyle = 'red'
ctx.fillRect(100, 60, 50, 50)
ctx.translate(100, 60)

ctx.rotate(1)
ctx.fillRect(0, 60, 100, 100)
```

![image-20210307212625855](https://gitee.com/MellowCo/BlobImg/raw/master/image-20210307212625855.png)

```js
ctx.save()
ctx.fillStyle = 'blue'
ctx.scale(2, 2)
// x y w h 都放大了2倍
ctx.fillRect(10, 10, 30, 30)

ctx.restore()
ctx.fillStyle = 'red'
ctx.fillRect(10, 10, 30, 30)
```

![image-20210307213538260](https://gitee.com/MellowCo/BlobImg/raw/master/image-20210307213538260.png)