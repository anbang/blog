render函数之JSX应用
# 一.模板缺陷

模板的最大特点是扩展难度大，不易扩展。可能会造成逻辑冗余,

比如做一个H1-H6的小组件

```
<Level :type="1">哈哈</Level>
<Level :type="2">哈哈</Level>
<Level :type="3">哈哈</Level>
```

Level组件需要对不同的type产生不同的标签

```
<template>
 <h1 v-if="type==1">
  <slot></slot>
 </h1>
 <h2 v-else-if="type==2">
  <slot></slot>
 </h2>
 <h3 v-else-if="type==3">
  <slot></slot>
 </h3>
</template>
<script>
export default {
 props: {
  type: {
   type: Number
  }
 }
};
</script>
```

# 二.函数式组件

> 函数式组件没有模板,只允许提供render函数

可以把复杂的逻辑变得非常简单

```
export default {
 render(h) {
  return h("h" + this.type, {}, this.$slots.default);
 },
 props: {
  type: {
   type: Number
  }
 }
};
```

# 三.JSX应用

使用jsx会让代码看起来更加简洁易于读取

上面的组件可以下面这样写

```
export default {
 render(h) {
  const tag = "h" + this.type;
  return <tag>{this.$slots.default}</tag>;
 },
 props: {
  type: {
   type: Number
  }
 }
};
```

# 四.render方法订制组件

编写List组件可以根据用户传入的数据自动循环列表



# 五.scope-slot

# 六.编写可编辑表格