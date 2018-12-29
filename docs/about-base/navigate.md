# 页面跳转

### 小程序页面间跳转

##### `wx.navigateTo(Object object)`

保留当前页面，跳转到应用内的某个页面。但是不能跳到 tabbar 页面。使用 wx.navigateBack 可以返回到原页面。

属性|类型|必填|说明
---|---|---|---
url|string|是|需要跳转的应用内非 tabBar 的页面的路径, 路径后可以带参数。参数与路径之间使用 ? 分隔，参数键与参数值用 = 相连，不同参数用 & 分隔；如 'path?key=value&key2=value2'
success|function|否|接口调用成功的回调函数
fail|function|否|接口调用失败的回调函数
complete|function|否|接口调用结束的回调函数（调用成功、失败都会执行）

```js
wx.navigateTo({
  url: 'test?id=1'
})
```

```js
// test/test.js
Page({
  onLoad(option) {
    console.log(option.query)
  }
})
```
<br>

##### `wx.switchTab(Object object)`

跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面

属性|类型|必填|说明
---|---|---|---
url|string|是|需要跳转的 tabBar 页面的路径（需在 app.json 的 tabBar 字段定义的页面），路径后不能带参数。
success|function|否|接口调用成功的回调函数
fail|function|否|接口调用失败的回调函数
complete|function|否|接口调用结束的回调函数（调用成功、失败都会执行）

```js
{
  "tabBar": {
    "list": [
      {
        "pagePath": "index",
        "text": "首页"
      },
      {
        "pagePath": "other",
        "text": "其他"
      }
    ]
  }
}
```

```js
wx.switchTab({
  url: '/index'
})
```

<br>
<br>

### 小程序跳转H5

小程序有一个web-view的组件，是一个可以用来承载网页的容器，会自动铺满整个小程序页面。

```html
home.wxml:
<view class="container">
   <navigator url="/pages/wxpage/wxpage">点击跳转到H5页面</navigator>
 </view>

wxpage.wxml：
<web-view src="https://www.tairanmall.com"></web-view>
```

在小程序`home.wxml`页面中，我们要跳到H5的`https://www.tairanmall.com`页面，在`home.wxml`，我们跳到一个专门用来跳转网页的容器页面`wxpage.wxml`。在这个页面中，有一个`web-view`组件，会自动铺满整个小程序页面。需要注意的是，src指向的链接，需要登录小程序管理后台配置业务域名。

<br>
<br>

### H5跳转小程序

H5跳转到小程序的方法，有两种：

1. 点击手机的返回键，让它自动根据层级返回
2. 使用JSSDK 1.3.2提供的接口返回小程序接口，需要在H5页面引入相应的js文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>H5跳转小程序</title>
</head>
<body>
  <h1 id="el">H5跳转小程序，请在小程序中打开</h1>
  <script type="text/javascript" src="https://res.wx.qq.com/open/js/jweixin-1.3.2.js"></script>
  <script>
    document.getElementById('el').addEventListener('click', function() {
      // 如果在微信小程序内,跳转到小程序的页面
      if (window.__wxjs_environment === 'miniprogram') { 
        // 跳转到小程序的页面，并传递了一个type=test的参数
        wx.miniProgram.navigateTo({url: '/pages/home/home?type=test' }) 
      } else {
        alert('不在小程序页面中')
      }
    })
  </script>
</body>
</html>
```
H5跳转小程序的参数直接拼接在URL后面，在跳转到小程序页面后，可以在`onload`的`options`中获取到传递过来的参数
