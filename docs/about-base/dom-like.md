# 节点操作

不能使用window.document对象，所以无法操作Dom，操作节点可以使用wx.createSelectorQuery()

### `SelectorQuery wx.createSelectorQuery()`

返回一个 SelectorQuery 对象实例。

在自定义组件或包含自定义组件的页面中，应使用 this.createSelectorQuery() 来代替。也可以使用`SelectorQuery SelectorQuery.in(Component component)`将选择器的选取范围更改为自定义组件 component 内。

```js
var query = wx.createSelectorQuery() // 创建节点查询器 query
query.select('#the-id').boundingClientRect()// 选择Id=the-id的节点，获取节点位置信息的查询请求
query.selectViewport().scrollOffset() // 获取页面滑动位置的查询请求
query.exec(function(res){
  res[0].top       // #the-id节点的上边界坐标
  res[1].scrollTop // 显示区域的竖直滚动位置
})
```

<br>
<br>


### `SeletorQuery`

##### `NodesRef SelectorQuery.select(string selector)`

在当前页面下选择第一个匹配选择器 selector 的节点。返回一个 NodesRef 对象实例，可以用于获取节点信息。

##### `NodesRef SelectorQuery.selectAll(string selector)`

在当前页面下选择匹配选择器 selector 的所有节点。

<br>

selector类似于 CSS 的选择器，但仅支持下列语法。

  * ID选择器：#the-id
  * class选择器（可以连续指定多个）：.a-class.another-class
  * 子元素选择器：.the-parent > .the-child
  * 后代选择器：.the-ancestor .the-descendant
  * 跨自定义组件的后代选择器：.the-ancestor >>> .the-descendant
  * 多选择器的并集：#a-node, .some-other-nodes

##### `NodesRef SelectorQuery.selectViewport()`

选择显示区域。可用于获取显示区域的尺寸、滚动位置等信息。

##### `NodesRef SelectorQuery.exec(function callback)`

执行所有的请求。请求结果按请求次序构成数组，在callback的第一个参数中返回。

<br>
<br>

### `NodesRef`

##### `SelectorQuery NodesRef.boundingClientRect(function callback)`
添加节点的布局位置的查询请求。相对于显示区域，以像素为单位。其功能类似于 DOM 的 getBoundingClientRect。返回 NodesRef 对应的 SelectorQuery。

属性|类型|说明
---|---|---
id|string|节点的 ID
dataset|Object|节点的 dataset
left|number|节点的左边界坐标
right|number|节点的右边界坐标
top|number|节点的上边界坐标
bottom|number|节点的下边界坐标
width|number|节点的宽度
height|number|节点的高度

```js
Page({
  getRect() {
    wx.createSelectorQuery().select('#the-id').boundingClientRect(function (rect) {
      rect.id // 节点的ID
      rect.dataset // 节点的dataset
      rect.left // 节点的左边界坐标
      rect.right // 节点的右边界坐标
      rect.top // 节点的上边界坐标
      rect.bottom // 节点的下边界坐标
      rect.width // 节点的宽度
      rect.height // 节点的高度
    }).exec()
  },
  getAllRects() {
    wx.createSelectorQuery().selectAll('.a-class').boundingClientRect(function (rects) {
      rects.forEach(function (rect) {
        rect.id // 节点的ID
        rect.dataset // 节点的dataset
        rect.left // 节点的左边界坐标
        rect.right // 节点的右边界坐标
        rect.top // 节点的上边界坐标
        rect.bottom // 节点的下边界坐标
        rect.width // 节点的宽度
        rect.height // 节点的高度
      })
    }).exec()
  }
})
```

##### `SelectorQuery NodesRef.scrollOffset(function callback)`
添加节点的滚动位置查询请求。以像素为单位。节点必须是 scroll-view 或者 viewport，返回 NodesRef 对应的 SelectorQuery。

属性|类型|说明
---|---|---
id|string|节点的 ID
dataset|Object|节点的 dataset
scrollLeft|number|节点的水平滚动位置
scrollTop|number|节点的竖直滚动位置

```js
Page({
  getScrollOffset() {
    wx.createSelectorQuery().selectViewport().scrollOffset(function (res) {
      res.id // 节点的ID
      res.dataset // 节点的dataset
      res.scrollLeft // 节点的水平滚动位置
      res.scrollTop // 节点的竖直滚动位置
    }).exec()
  }
})
```

##### `SelectorQuery NodesRef.context(function callback)`

添加节点的 Context 对象查询请求。目前支持 VideoContext、CanvasContext、LivePlayerContext 和 MapContext 的获取。

属性|类型|说明
---|---|---
context|Object|节点对应的 Context 对象

```js
Page({
  getContext() {
    wx.createSelectorQuery().select('.the-video-class').context(function (res) {
      console.log(res.context) // 节点对应的 Context 对象。如：选中的节点是 <video> 组件，那么此处即返回 VideoContext 对象
    }).exec()
  }
})
```

##### `NodesRef.fields(Object fields)`

获取节点的相关信息。需要获取的字段在fields中指定。返回值是 nodesRef 对应的 selectorQuery

属性|类型|默认值|必填|说明
id|boolean|false|否|是否返回节点 id
dataset|boolean|false|否|是否返回节点 dataset
rect|boolean|false|否|是否返回节点布局位置（left right top bottom）
size|boolean|false|否|是否返回节点尺寸（width height）
scrollOffset|boolean|false|否|是否返回节点的 scrollLeft scrollTop，节点必须是 scroll-view 或者 viewport
properties|Array.<string>|[]|否|指定属性名列表，返回节点对应属性名的当前属性值（只能获得组件文档中标注的常规属性值，id class style 和事件绑定的属性值不可获取）
computedStyle|Array.<string>|[]|否|指定样式名列表，返回节点对应样式名的当前值
context|boolean|false|否|是否返回节点对应的 Context 对象

```js
Page({
  getFields() {
    wx.createSelectorQuery().select('#the-id').fields({
      dataset: true,
      size: true,
      scrollOffset: true,
      properties: ['scrollX', 'scrollY'],
      computedStyle: ['margin', 'backgroundColor'],
      context: true,
    }, function (res) {
      res.dataset // 节点的dataset
      res.width // 节点的宽度
      res.height // 节点的高度
      res.scrollLeft // 节点的水平滚动位置
      res.scrollTop // 节点的竖直滚动位置
      res.scrollX // 节点 scroll-x 属性的当前值
      res.scrollY // 节点 scroll-y 属性的当前值
      // 此处返回指定要返回的样式名
      res.margin
      res.backgroundColor
      res.context // 节点对应的 Context 对象
    }).exec()
  }
})
```

##### `SelectorQuery NodesRef.context(function callback)`

添加节点的 Context 对象查询请求。目前支持 VideoContext、CanvasContext、LivePlayerContext 和 MapContext 的获取。

```js
Page({
  getContext() {
    wx.createSelectorQuery().select('.the-video-class').context(function (res) {
      console.log(res.context) // 节点对应的 Context 对象。如：选中的节点是 <video> 组件，那么此处即返回 VideoContext 对象
    }).exec()
  }
})
```
