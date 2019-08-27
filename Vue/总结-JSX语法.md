render 函数之 JSX 应用
* 一。模板缺陷
* 二。函数式组件
* 三。JSX 应用
* 四。render 方法订制组件
* 五。scope-slot
* 六。编写可编辑表格

# 一。模板组件的缺陷

文件是`.vue`文件，模板的最大特点是扩展难度大，不易扩展。可能会造成逻辑冗余

```
<Level :type="1">哈哈</Level>
<Level :type="2">哈哈</Level>
<Level :type="3">哈哈</Level>
```

Level 组件需要对不同的 type 产生不同的标签

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

# 二。函数式组件

文件是`.js`文件，函数式组件没有模板，只允许提供 render 函数，可以把复杂的逻辑变得非常简单

当时创建 html 时候还是有点麻烦，所以就有`jsx`了

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

# 三。JSX 应用

使用 `jsx` 会让代码看起来更加简洁易于读取

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

# 四。render 方法订制组件

编写 List 组件可以根据用户传入的数据自动循环列表

```html
<!-- App.vue -->
<List :data="data"></List>
<script>
import List from "./components/List";
export default {
 data() {
  return { data: ["苹果", "香蕉", "橘子"] };
 },
 components: {
  List
 }
};
</script>

<!-- List组件渲染列表 -->
<template>
 <div class="list">
  <div v-for="(item,index) in data" :key="index">
   <li>{{item}}</li>
  </div>
 </div>
</template>
<script>
export default {
 props: {
  data: Array,
  default: () => []
 }
};
</script>
```

## 通过 render 方法来订制组件，在父组件中传入 render 方法

```html
<template>
  <div>
    <List :data="data" :render="render"></List>
  </div>
</template>

<script>
import List from "./components/List.vue";
export default {
  data() {
    return {
      data: ["苹果", "香蕉", "橘子"]
    };
  },
  components: {
    List
  },
  methods: {
    //执行这里的render的函数来渲染自定义的
    render(h, name) {
      return <span class="diy-class">{name}</span>;
    }
  }
};
</script>
```

我们需要 createElement 方法，就会想到可以编写个函数组件，将 createElement 方法传递出来

```html
<template>
  <div>
    <div class="list" v-for="(item,index) in data" :key="index">
      <template v-if="render">
        <!-- 将render方法传到函数组件中，将渲染项传入到组件中，在内部回调这个render方法 -->
        <ListItem :render="render" :item="item"></ListItem>
      </template>
      <h1 v-else>{{item}}</h1>
    </div>
  </div>
</template>

<script>
import ListItem from "./ListItem.vue";

export default {
  props: {
    data: Array,
    render: Function
  },
  components: {
    ListItem
  }
};
</script>
```

ListItem.vue 调用最外层的 render 方法，将 createElement 和当前项传递出来

```html
<script>
export default {
  props: {
    render: {
      type: Function
    },
    item: {}
  },
  render(h) {
    return this.render(h, this.item);
  }
};
</script>
```

# 五。scope-slot

??????????????????????

使用 v-slot 将内部值传即可

```html
<List :arr="arr">
    <template v-slot="{item}">
        {{item}}
    </template>
</List>

<div v-for="(item,key) in arr" :key="key">
    <slot :item="item"></slot>
</div>
```

# 六。编写可编辑表格

基于 iview 使用 jsx 扩展成可编辑的表格

```html
<template>
<div>
  <Table :columns="columns" :data="data"></Table>
</div>
</template>
<script>
import Vue from 'vue';
export default {
  methods:{
    render(h,{column,index,row}){
      let value = row[column.key];
      return <div on-click={(e)=>this.changeIndex(e,index)} >
      {this.index === index?
        <i-input type="text" value={value} on-input={(value)=>{
          this.handleChange(value,column,row)
        }} onOn-enter={()=>this.enter(row,index)}/>:
        <span>{value}</span>
      }
      </div>
    },
    enter(row,index){
      this.data.splice(index,1,row);
      this.index = -1;
    },
    handleChange(value,column,row){
      row[column['key']]= value;
    },
    changeIndex(e,index){
      this.index = index;
      this.$nextTick(()=>{
        e.currentTarget.getElementsByTagName("input")[0].focus()
      })
    }
  },
  data() {
    return {
      index:-1,
      columns: [
        {
          title: 'Name',
          key: 'name',
          render:this.render
        },
        {
          title: 'Age',
          key: 'age',
        },
        {
          title: 'Address',
          key: 'address',
        },
      ],
      data: [
        {
          name: 'John Brown',
          age: 18,
          address: 'New York No. 1 Lake Park',
          date: '2016-10-03',
        },
        {
          name: 'Jim Green',
          age: 24,
          address: 'London No. 1 Lake Park',
          date: '2016-10-01',
        },
        {
          name: 'Joe Black',
          age: 30,
          address: 'Sydney No. 1 Lake Park',
          date: '2016-10-02',
        },
        {
          name: 'Jon Snow',
          age: 26,
          address: 'Ottawa No. 2 Lake Park',
          date: '2016-10-04',
        },
      ],
    };
  },
};
</script>

```
