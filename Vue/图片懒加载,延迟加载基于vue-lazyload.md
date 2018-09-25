vue做手机项目的时候，懒加载是非常常用的；

效果是默认不加载图片，先用一个占位符图来代替，等使用图片的时再进行加载（比如滚动到图片的时候），如果真正的图片请求出错了，用默认的出错图片来进行占位；

使用的是：https://github.com/hilongjw/vue-lazyload#requirements

这个vue-lazyload的用法还是很简单的，我用的是下这个版本（当前最新）

`"vue-lazyload":"^1.0.5",`

在main.js里进行引入和使用

```vue
import VueLazyload from 'vue-lazyload'
 
Vue.use(VueLazyload, {
  preLoad: 1.3,
  error: require('@/assets/img/dou_dou.jpg'),
  loading: require('@/assets/img/dou_dou.jpg'),
  attempt: 1
})

```
上面做里基础配置；这样就可以全局使用的里；

比如我的列表图片是通过设置区域背景的方式，

``` 
<div class="cover"
     style="position: relative; background: center center / cover no-repeat rgb(250, 250, 250);"
     v-lazy:background-image="item.target.cover_url"
     v-if="item.target.cover_url"
>
  <div style="padding-top: 100%"></div>
</div>
```
这样就OK了。核心是加

``` 
v-lazy

```
具体看它的readme介绍

```vue
<script>
export default {
  data () {
    return {
      imgObj: {
        src: 'http://xx.com/logo.png',
        error: 'http://xx.com/error.png',
        loading: 'http://xx.com/loading-spin.svg'
      },
      imgUrl: 'http://xx.com/logo.png' // String
    }
  }
}
</script>
 
<template>
  <div ref="container">
     <img v-lazy="imgUrl"/>
     <div v-lazy:background-image="imgUrl"></div>
     
     <!-- with customer error and loading -->
     <img v-lazy="imgObj"/>
     <div v-lazy:background-image="imgObj"></div>
     
     <!-- Customer scrollable element -->
     <img v-lazy.container ="imgUrl"/>
     <div v-lazy:background-image.container="img"></div>
  
    <!-- srcset -->
    <img v-lazy="'img.400px.jpg'" data-srcset="img.400px.jpg 400w, img.800px.jpg 800w, img.1200px.jpg 1200w">
    <img v-lazy="imgUrl" :data-srcset="imgUrl' + '?size=400 400w, ' + imgUrl + ' ?size=800 800w, ' + imgUrl +'/1200.jpg 1200w'" />
  </div>
</template>
```