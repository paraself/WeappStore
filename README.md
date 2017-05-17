# WeappStore
 一个超轻量级的微信小程序状态管理库  
An super lightweight state management tool for Weapp.

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
// 可以开启debug模式
store.debug = true
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
    app.$store.link(this, 'userName')
  }
})
```
```html
<view class="Page">
 Username: {{userName}}
</view>
```
4. 好了，现在如果需要改变全局状态变量的时候，就可以这么写啦：
```javascript
  app.$store.setState('userName', 'Alice')
```
所有绑定这个全局状态变量的页面，都会自动更新啦。

5. 同时在页面的wxml也可以直接这么写
```javascript
<view data-state=" userName: {{someVariable}} " catchtap="setState" />
```
因为在将页面link的时候，就自动为页面添加了setState方法，同时约定，data-state可以用于传递state的值。但是这里有一个限制，就是data-state只能是下面几种形式：
```javascript
data-state=" userName: Alice "

{
 userName: 'Alice'
}

data-state=" userNumber: 15 "
{
 userNumber: 15
}

date-state=" userBool: false"
{
 userBool: false
}
```
`string`，`number`和`boolean`可以直接解析成对应的类型。

下面是几种错误的写法：
```javascript
date-state=" {{userBool: myBoolVariable}}"
date-state=" userName: {{myNameVariable}}, userEmail: {{email}}"
```
目前在data-state里，仅支持一对键-值，如果需要设置更多的，那么可以在page里在写一个方法，在方法里再去设置state。


# 很简单！有没有！
再重复一下，只有三个api
```javascript
// 创建store实例
var weappStore = new WeappStore(stateObject)
// 将全局状态变量绑定到页面, 如果状态名空缺的话，则只会把为页面赋予setState的方法，这样页面相当于只能设置state。
weappStore.link(page, stateName='')
// 更新全局变量
weappStore.setState(stateName, newValue)
```
Redux什么的，暂时先放放吧。
