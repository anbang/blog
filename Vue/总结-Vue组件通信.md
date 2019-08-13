Vue 组件间通信方式

# 快速原型开发

可以快速识别。vue 文件封装组件插件等功能

```
npm install @vue/cli -g
npm install -g @vue/cli-service-global
vue serve App.vue
```

# 一 Props 传递数据 和 $emit 触发

```
components
   ├── Grandson1.vue // 孙子1
   ├── Grandson2.vue // 孙子2
   ├── Parent.vue   // 父亲
   ├── Son1.vue     // 儿子1
   └── Son2.vue     // 儿子2
```

在父组件`Parent.vue`中使用儿子组件；

下面是普通用法

父组件如下：

```
<template>
  <div>
    父组件:{{mny}}
    <Son1 :mny="mny" @input="change"></Son1>
  </div>
</template>
<script>
import Son1 from "./Son1";
export default {
  data() {
    return { mny: 100 };
  },
  components: {
    Son1
  },
  methods: {
    change(mny) {
      console.log("App change");
      this.mny = mny;
    }
  }
};
</script>
```

子组件如下：

```
<template>
  <div>
    <h1>Hello, Son1!{{mny}}</h1>
    <button @click="$emit('input',200)">变</button>
  </div>
</template>

<script>
export default {
  props: {
    mny: {
      type: Number
    }
  }
};
</script>
```

# 二 $emit 触发

下面是同步父子组件的数据，语法糖的写法；

一个是`.sync`, 一个是`v-model`，这两种写法只需要提供值即可；

子模块可以使用`'update:xxx'` 的语法或者 `"$emit('input',200)"`这种用法；

## .sync 写法

这种写法，

父组件用法：提供值即可，不需要有处理的方法；使用`<Son1 :mny.sync="mny"></Son1>`

子组件用法：`<button @click="$emit('update:mny',200)">变</button>` 使用 `'update:mny'` 的语法；

---

父组件

```
<template>
  <div>
    父组件:{{mny}}
    <!-- 1.普通传值 -->
    <!-- <Son1 :mny="mny" @input="change"></Son1> -->

    <!-- 2.sync -->
    <Son1 :mny.sync="mny"></Son1>
  </div>
</template>
<script>
import Son1 from "./Son1";
export default {
  data() {
    return { mny: 100 };
  },
  components: {
    Son1
  }
};
</script>
```

子组件

```
<template>
  <div>
    <h1>Hello, Son1!{{mny}}</h1>
    <!-- 1.普通传值 -->
    <!-- <button @click="$emit('input',200)">变</button> -->

    <!-- 2.sync -->
    <button @click="$emit('update:mny',200)">变</button>
  </div>
</template>

<script>
export default {
  props: {
    mny: {
      type: Number
    }
  }
};
</script>
```

## v-model

父组件：提供值即可，不需要有处理的方法；使用`<Son1 v-model="mny"></Son1>`

子组件：父级传的值只能通过固定的`value`来接受，触发只能触发`input`事件，`<button @click="$emit('input',200)">更改{{value}}</button>`

```
<template>
  <div>
    父组件:{{mny}}
    <!-- 1.普通传值 -->
    <!-- <Son1 :mny="mny" @input="change"></Son1> -->

    <!-- 2.sync -->
    <!-- <Son1 :mny.sync="mny"></Son1> -->

    <!-- 3.v-model -->
    <Son1 v-model="mny"></Son1>
  </div>
</template>
<script>
import Son1 from "./Son1";
export default {
  data() {
    return { mny: 100 };
  },
  components: {
    Son1
  }
};
</script>
```

```
<template>
  <div>
    <h1>Hello, Son1：{{value}}</h1>
    <!-- 1.普通传值 -->
    <!-- <button @click="$emit('input',200)">变</button> -->

    <!-- 2.sync -->
    <!-- <button @click="$emit('update:mny',200)">变</button> -->

    <!-- 3.v-model -->
    <button @click="$emit('input',200)">更改{{value}}</button>
  </div>
</template>
<script>
export default {
  props: {
    // mny: {
    //   type: Number
    // }
    value: {
      // 3.v-model 接收到的属性名只能叫value
      type: Number
    }
  }
};
</script>
```

# 三。$parent、$children

`父模块` 引用 `子模块`；`子模块` 引用 `孙模块`.....;

这种情况`孙模块`可以通过`$parent`来触发`父模块`

```
<!-- 子模块引用孙模块 -->
<Grandson1 :value="value"></Grandson1>

<!-- 孙模块 -->
<template>
    <div>
        <h1>Hello, Grandson1==== {{value}}</h1>
        <button @click="$parent.$emit('input',200)">更改</button>
    </div>
</template>

<script>
    export default {
        props: {
            value: {
                type: Number
            }
        }
    };
</script>
```

> 如果层级很深那么就会出现 $parent.$parent.....
> 我们可以封装一个 $dispatch 方法向上进行派发
> 既然能向上派发那同样可以向下进行派发

## $dispatch

自定义`dispatch`, 向上派发

```
Vue.prototype.$dispatch = function $dispatch(eventName, data) {
  let parent = this.$parent;
  while (parent) {
    parent.$emit(eventName, data);
    parent = parent.$parent;
  }
};
```

## $broadcast

自定义`broadcast`, 向下进行派发

```
Vue.prototype.$broadcast = function $broadcast(eventName, data) {
  const broadcast = function () {
    this.$children.forEach((child) => {
      child.$emit(eventName, data);
      if (child.$children) {
        $broadcast.call(child, eventName, data);
      }
    });
  };
  broadcast.call(this, eventName, data);
};
```

# 四。$attrs、$listeners

当批量拿属性和方法的时候（比如子组件批量向下传从父级接受的属性 / 方法），可以使用 `$attrs`、`$listeners`来拿

## $attrs

`$attrs`批量向下传入属性

`$listeners`批量向下传入方法

```
<Son2 name="anbang" age="29" @click="()=>{this.mny=500}"></Son2>

<!-- son2 -->
<template>
  <div>
    <h1>Hello {{$attrs.name}}, Son2 {{$attrs}}</h1>
    <Grandson2 v-bind="$attrs" v-on="$listeners"></Grandson2>
  </div>
</template>
<script>
import Grandson2 from "./Grandson2";
export default {
  components: {
    Grandson2
  }
};
</script>

<!-- Grandson2 -->
<template>
  <div>
    <h2>Hello, Grandson2{{$attrs}}</h2>
    <button @click="$listeners.click()">更改</button>
  </div>
</template>
```

# 五。Provide & Inject

父级模块暴露自己，然后在子模块注入父级数据

> 注意：父级模块暴露自己，只能子孙模块注入；非子孙模块不用侏儒

> Provide 在父级中暴露数据
> Inject : 在任意子组件中可以注入父级数据

```
<!-- parent 模块暴露 -->
provide() {
    return {
        parentMsg: "父亲 Parent123"
    };
}

<!-- son1 模块暴露 -->
  provide() {
    return {
      sonMsg: "son1Msg 信息"
    };
  }

  <!-- Grandson1模块引用 -->
  inject: ["parentMsg", "sonMsg"]
```

```
inject: ["parentMsg"] // 会将数据挂载在当前实例上
```

# 六。Ref 使用

获取组件实例

```
<Grandson2 v-bind="$attrs" v-on="$listeners" ref="anbang"></Grandson2>
mounted() { // 获取组件定义的属性
  console.log(this.$refs.anbang.name);
}
```

# 七。EventBus

EventBus 又称为事件总线。在 Vue 中可以使用 EventBus 来作为沟通桥梁的概念，就像是所有组件共用相同的事件中心，可以向该中心注册发送事件或接收事件，所以组件都可以上下平行地通知其他组件，但也就是太方便所以若使用不慎，就会造成难以维护的灾难，因此才需要更完善的 Vuex 作为状态管理中心，将通知的概念上升到共享状态层次。

**用于跨组件通知** 不复杂的项目可以使用这种方式，跨组件通信复杂的就用 Vuex 吧

```
Vue.prototype.$bus = new Vue();
```

Son2 组件和 Grandson1 相互通信

```
<!-- Son2 -->
 mounted() {
  this.$bus.$on("my", data => {
   console.log(data);
  });
 },
```

```
<!-- Grandson1 -->
mounted() {
  this.$nextTick(() => {
   this.$bus.$emit("my", "我是Grandson1");
  });
 },
```

# 八。Vuex 通信

![https://www.fullstackjavascript.cn/vuex.png](<https://www.fullstackjavascript.cn/vuex.png>)