# WeappStore
An super lightweight state management tool for Weapp. 一个超轻量级的微信小程序状态管理库

Redux太复杂了，有没有！
好吧，其实我只想同步一下全局变量所绑定的视图而已。那么试试这个轻量的状态管理工具把。

## Usage
1. 先下载WeappStore.js，并把它放在根目录下的utils目录里。
2. 在app.js里，先初始化WeappStore，并把它存在app上。
```javascript
var WeappStore = require('./utils/store.js')
// 创建store实例，并定义state，也就是希望在全局使用的状态变量。
var store = new WeappStore({
  userName: 'Rebecca',
  userEmail: undefined
})
// 把store存在app上
App({
  onLaunch: function () {},
  onError: function () {},
  $store: store
})
```
3. 在需要使用全局状态变量的页面的onLoad函数里，将页面变量和全局状态变量进行连接。
```javascript
var app = getApp()
Page({
  data: {},
  onLoad: function () {
    app.$store.link('userName', this)
  }
})
```
4. 好了，现在如果需要改变全局状态变量的时候，就可以这么写啦：
```javascript
  app.$store.setState('userName', 'Alice')
```
所有绑定这个全局状态变量的页面，都会自动更新啦。

# 很简单！有没有！
再重复一下，只有三个api
```javascript
// 创建store实例
var weappStore = new WeappStore(stateObject)
// 将全局状态变量绑定到页面
weappStore.link(stateName, page)
// 更新全局变量
weappStore.setState(stateName, newValue)
```
Redux什么的，暂时先放放吧。
