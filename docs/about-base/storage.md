# 缓存

每个微信小程序都可以有自己的本地缓存，可以通过 wx.setStorage/wx.setStorageSync、wx.getStorage/wx.getStorageSync、wx.clearStorage/wx.clearStorageSync，wx.removeStorage/wx.removeStorageSync 对本地缓存进行读写和清理。

同一个微信用户，同一个小程序 storage 上限为 10MB。storage 以用户维度隔离，同一台设备上，A 用户无法读取到 B 用户的数据。

注意： 如果用户储存空间不足，我们会清空最近最久未使用的小程序的本地缓存。我们不建议将关键信息全部存在 storage，以防储存空间不足或用户换设备的情况。

### 写入缓存

`wx.setStorage(Object object)`

将数据存储在本地缓存中指定的 key 中。会覆盖掉原来该 key 对应的内容。数据存储生命周期跟小程序本身一致，即除用户主动删除或超过一定时间被自动清理，否则数据都一直可用。单个 key 允许存储的最大数据长度为 1MB，所有数据存储上限为 10MB。

属性|类型|必填|说明
---|---|---|---
key|string|是|本地缓存中指定的 key
data|any|是|需要存储的内容。只支持原生类型、Date、及能够通过JSON.stringify序列化的对象。	
success|function|否|接口调用成功的回调函数	
fail|function|否|接口调用失败的回调函数	
complete|function|否|接口调用结束的回调函数（调用成功、失败都会执行）

```js
wx.setStorage({
  key: 'key',
  data: 'value'
})
```

`wx.setStorageSync(string key, any data)`: `wx.setStorage` 的同步版本

```js
try {
  wx.setStorageSync('key', 'value')
} catch (e) { }
```
<br>
<br>

### 读取缓存

`wx.getStorage(Object object)`

从本地缓存中异步获取指定 key 的内容

属性|类型|必填|说明
----|---|---|---
key|string|是|本地缓存中指定的 key
success|function|否|接口调用成功的回调函数
fail|function|否|接口调用失败的回调函数
complete|function|否|接口调用结束的回调函数（调用成功、失败都会执行）

```js
wx.getStorage({
  key: 'key',
  success(res) {
    console.log(res.data)
  }
})
```

`any wx.getStorageSync(string key)`: `wx.getStorage 的同步版本`

```js
try {
  const value = wx.getStorageSync('key')
  if (value) {
    // Do something with return value
  }
} catch (e) {
  // Do something when catch error
}
```
<br>
<br>

### 清理缓存

`wx.clearStorage(Object object)`

清理本地数据缓存

属性|类型|必填|说明
---|---|---|---
success|function|否|接口调用成功的回调函数
fail|function|否|接口调用失败的回调函数
complete|function|否|接口调用结束的回调函数（调用成功、失败都会执行）

```js
wx.clearStorage()
```

`wx.clearStorageSync()`:`wx.clearStorage` 的同步版本

```js
try {
  wx.clearStorageSync()
} catch (e) {
  // Do something when catch error
}
```

<br>
<br>

### 移除缓存

`wx.removeStorage(Object object)` 

从本地缓存中移除指定 key


属性|类型|必填|说明
---|---|---|---
key|string|是|本地缓存中指定的 key
success|function|否|接口调用成功的回调函数
fail|function|否|接口调用失败的回调函数
complete|function|否|接口调用结束的回调函数（调用成功、失败都会执行）

```js
wx.removeStorage({
  key: 'key',
  success(res) {
    console.log(res.data)
  }
})
```

`wx.removeStorageSync(string key)`: `wx.removeStorage` 的同步版本

```js
try {
  wx.removeStorageSync('key')
} catch (e) {
  // Do something when catch error
}
```