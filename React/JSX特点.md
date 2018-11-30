### 知识点：
- JSX的本质
- 如何使用JSX
- JSX优点

# 一、JSX的本质：动态创建组件的语法糖
JSX在JS代码中直接写HTML标记
```javascript
const name = "zhu";
const ele = <h1>HelLo,{name}</h1>;
```
JSX并不是模板语言，而是一种语法糖；写更简洁的代码，做繁琐的事情；

上面的代码，本质是这样的

```javascript
const name = "zhu";
const ele = React.createElement(
    'h1',       //标记类型
    null,       //属性
    'Hello,',   //chilren,以后都是chilren
    name        //chilren
);
```
### 复杂的组件转换

下面是一个评论组件的JSX写法；
```javascript
class CommentBox extends React.Component {
    render (){
        return (
            <div className="comments">
                <h1>Comments ({this.state.items.length})</h1>
                <CommentList data={this.state.items}/>
                <CommentForm />
            </div>
        );
    }
}
ReactDOM.render(<CommentBox topicOd="1"/>,mountNode);
```
转换为JS的写法如下
```javascript
class CommentBox extends React.Component {
    render (){
        return React.createElement(
            'div',
            {className:"comments"},
            React.createElement(
                'h1',
                null,
                'Comments('
                this.state.items.length,
                ')'
            ),
            React.createElement(CommentList,{data:this.state.items}),
            React.createElement(CommentForm,null),
        )
    }
}
ReactDOM.render(React.createElement(CommentBox,{topicOd:"1"}),mountNode);
```

# 二、如何使用JSX
- JSX本身就是表达式
    ```javascript
    const element = <h1>Hello,world</h1>;
    ```
- 在属性中使用表达式
    ```javascript
    <MyCompoent foo={1+2+3+4} />;
    ```
- 延展属性
    ```javascript
    const props = {firstName:'Ben',lastName:'Hector'};
    const greeting = <Greeting {...props}/>;
    ```
- 表达式作为子元素
    ```javascript
    const element = <li>{props.message}</li>
    ```

# 三、JSX优点
JSX优点
- 创建方式，非常直观
- 代码创建灵活
- 无需学习新的模板语言

### 约定：自定义组件以大写子母开头
- React认为小写的tag是原生的DOM节点；比如`div`；
- 大写子母开头为自定义组件；
- JSX标记可以直接使用属性的语法,这种方式不需要遵守大写子母开头的约定;例如`<menu.item />`