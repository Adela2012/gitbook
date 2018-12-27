# wxs使用

`WXS（WeiXin Script）`是小程序的一套脚本语言，结合`WXML`，可以构建出页面的结构。

`wxs`可以说就是为了满足能在页面中使用`js`存在的，在`wxml`页面中，只能在插值`{{ }}`中写简单的`js`表达式，而不能调用方法，例如想获得某个时间的年份。

虽然也可以在`page`的`data`对象中先把这个年份定义好赋给某个变量，然后在页面中使用这个变量，但是如果这样的变量多了，代码就会很臃肿，可读性不高，后续维护也麻烦。

相对来说`wxs`就是弥补了这样的短处。

注意
- wxs 不依赖于运行时的基础库版本，可以在所有版本的小程序中运行。
- wxs 与 javascript 是不同的语言，有自己的语法，并不和 javascript 一致。
- wxs 的运行环境和其他 javascript 代码是隔离的，wxs 中不能调用其他 javascript 文件中定义的函数，也不能调用小程序提供的API。
- wxs 函数不能作为组件的事件回调。
- 由于运行环境的差异，在 iOS 设备上小程序内的 wxs 会比 javascript 代码快 2 ~ 20 倍。在 android 设备上二者运行效率无差异。

<br>

在 filters.wxs 文件中

```js
// filters.wxs
var getYear = function (date) {
    date = getDate(date) || getDate()
    return date.getFullYear()
  },

module.exports = {
  getYear: getYear
}

```

在 trade/trade.wxml 文件中

```html
<!-- trade/trade.wxml -->
<wxs module="filters" src="filters.wxs"></wxs>
<view class="year-title" >{{filters.getYear(tradeTime)}}年</view>
```

从上述代码中，可以看到wxs语言的写法与js差不多，基本上我们在写的时候，可以沿用js的写法，但是个别细节部分也需要注意。例如wxs的`getDate(date)` 和 js的`new Date(date)`

WXS 语言目前共有以下几种数据类型：
- number ： 数值
- string ：字符串
- boolean：布尔值
- object：对象
- function：函数
- array : 数组
- date：日期
- regexp：正则

在使用中，我们可以阅读官方文档查看具体语法[wxs数据类型](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxs/06datatype.html)


