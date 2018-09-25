我的使用场景：

- 父组件：home.vue
- 子组件：feed-section.vue

父组件home.vue通过请求拿到数据后，传递给子组件feed-section.vue；因为在子组件中需要渲染HTML；（当然如果简单些你可以使用vue插槽；）

为了让组件完全的可复用；尽量不在父组件干涉子组件内容

实现方法：

父组件中的操作：把数据放在子组件的 :xxxxx上，这样就传过去了

``` 
<feed-section :recommend_feeds="recommend_feeds"></feed-section>
```


子附件的操作：

``` 
props: [
  'recommend_feeds'
],
```

通过props中写一个`recommend_feeds` 这样就可以拿到父组件通过`recommend_feeds`穿过来的数据了；

具体写法如下图

### home.vue的

### feed-section.vue中的代码

