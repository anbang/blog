在做微信小程序页面跳转的时候；

有两个常用的方法；

wx.navigateTo 和 wx.redirectTo

wxml文件如下

```
<view class="container">
  <view   class="userinfo">
    <image class="userinfo-avatar" src="{{userInfo.avatarUrl}}" background-size="cover"></image>
    <text class="userinfo-nickname">{{userInfo.nickName}}</text>
  </view>
  <view class="usermotto" bindtap="bindViewTap">
    <text class="user-motto">{{motto}}</text>
  </view>
</view>

```

js文件

```javascript
//index.js 获取应用实例
var app = getApp();
Page({
  data: {
    motto: '立即体验',
    userInfo: {}
  },
  //事件处理函数
  bindViewTap: function() {
    // wx.navigateTo({
    //   url: '../logs/logs'
    // })
    wx.redirectTo({
      url: '../home/index'
    })
  },
  onLoad: function () {
    console.log('onLoad');
    console.log(app.globalData.userInfo);
    var that = this;
    //调用应用实例的方法获取全局数据
    app.getUserInfo(function(userInfo){
      //更新数据
      that.setData({
        userInfo:userInfo
      })
    })
  }
})
```

点击后，可以使用两种跳转；

`wx.navigateTo` 和 `wx.redirectTo`

### wx.navigateTo

这个不会销毁当前页面，触发了源页面的 `onHide` 事件；而且左上角会有返回字样，还可以点击回去；

或者使用 `wx.navigateBack` 返回到原页面。

### wx.redirectTo

销毁当前的页面，触发源页面的 `onUnload` 事件；而且左上角空的，无返回