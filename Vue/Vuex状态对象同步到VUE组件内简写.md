把Vuex内的一个状态对象的属性，简写；

一共两种方式
- Vuex的基础搭建
- mapState辅助

DEMO如下；

# 一、Vuex的基础搭建

## 1.src下面创建store.js，文件内容如下

```javascript
import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex)

const options={
    //状态对象
    state: {
        count: 111
    },
    //触发状态
    mutations: {
        increment (state) { 
            state.count++ 
        },
        jian(state){ 
            state.count– 
        }
    }
}
const store =new Vuex.Store(options)
export default store;
```

## 2.mainjs中引用

```javascript
import Vue from 'vue';
import App from './App';
import router from './router';
import store from './store';

import { create } from 'domain';
Vue.config.productionTip = false
/* eslint-disable no-new */
new Vue({
    el: "#app",
    router,
    store,
    render:h=>h(App)
})
```

## 3.在Vue文件中使用

`<p>Vuex的数据 {{$store.state.count}}</p>`

下面开始正式做数据同步；

# 一、computed 动态计算

```javascript
computed:{
    detailCount(){
        return this.$store.state.count
    }
},
```

在vue文件中直接使用组件内的数据即可；

`<p>内部的数据 {{detailCount}}</p>`

# 二、mapState辅助；

在单独构建的版本中辅助函数为 `Vuex.mapState` 在组件内，先import；

```javascript
import { mapState } from ‘vuex’
computed:mapState({
    count : state => state.count
})
```
这样即可；还可以做下面这种简化的写法

```javascript
computed:mapState(['count'])
```

下面这种方法，还是非常常用的；

如果想连 `mutatios` 里面的方法也一起简写；

可以下面这样；

```javascript
import { mapState ,mapMutations } from 'vuex'
 
export default {
  name:"UserDetail",
  data(){
      return{
          logo:"AXIHE"
 
      }
  },
  computed: mapState(['count']),
  methods:mapMutations([
      'increment',
      'jian'
  ]),
  watch:{
      '$route'(to,form){
          console.log(to)
          console.log(form)
      }
  }
}
```
这样的话，就可以直接使用了；